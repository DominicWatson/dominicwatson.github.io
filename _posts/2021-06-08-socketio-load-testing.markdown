--- 
name: socketio-load-testing
layout: post
title: "Load testing Socket.IO with Artillery + Kubernetes and sticky sessions"
time: 2021-06-08 16:38:00 +00:00
---

[Artillery.io](https://artillery.io/) is a nifty load testing framework written in node.js. Test scenarios are written in `yaml` files with the option for custom functionality written in js.

One of its really useful features is its ability to generate [socket.io]() websocket traffic out of the box without having to hand code all the network handshaking that comes with it. For example, a minimal test:

```yaml
config:
  target: "https://mysite.com"
  phases:
    - duration: 600
      arrivalRate: 1
      rampTo: 1
scenarios:
  - name: "Very simple test"
    engine: "socketio"
    flow:
      - loop:
        - emit:
            channel: "msg"
            data: "hello world! This is a random string: {{ $randomString() }}"
            namespace: "/simpleloadtest"
        - think: 10
        count: 50
```

_(I've yet to find any working examples with Gatling and socket.io - there certainly is nothing out of the box as far as I can see)._

## Problems with testing against load balanced in Kubernetes app

We use Kubernetes and the official NGiNX ingress controller. For various reasons, when running a multi-replica application, we have sticky sessions enabled. These sticky sessions work by the ingress controller setting a `route` cookie for new traffic that "sticks" it to one of the running pods of the application. This works a treat when connecting to the socket.io server from a browser.

When running the Artillery tests, however, these cookies are not sent back with subsequent requests. This means that the socket.io handshake calls fail because they get spread across multiple pods, rather than sticking to a single pod.

This is caused by the socket.io client library for node which will not resend the cookies sent back due to cross domain security. And there's nothing we can do about that.

## Workaround

To have your load test scenarios stick on a single pod, you need to set the `route`cookie manually. This can be achieved using `socketio` config options in the test scenario yaml:

```yaml
scenarios:
  - name: "Very simple test"
    engine: "socketio"
    socketio:
        extraHeaders:
            Cookie: "route=349859.3499.345804.439"
    # ...
```

Ideally, this cookie will be set dynamically from actual server responses rather than be hand coded like this. However, this is not possible right now (as of Artillery 1.7.2). What we _can_ do however, is create a variable that is an array of route IDs and use those to spread the load across your replicas:

```yaml
config:
  variables:
    # this weird hack gives us a bunch of possibilities to route to different servers and seemingly stick with
    # them (this is a quirk of kubernetes nginx ingress load balancing it seems)
    route:
      - a
      - b
      - c
      - d
      # etc.
  # ...
scenarios:
  - name: "Very simple test"
    engine: "socketio"
    socketio:
        extraHeaders:
            Cookie: "route={{ route }}"
```

Et voila. Hopefully that avoids some head scratching. Further info an conversation can be found here:

[https://github.com/artilleryio/artillery/discussions/1061](https://github.com/artilleryio/artillery/discussions/1061)