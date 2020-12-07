--- 
name: gethttprequestdata
layout: post
title: GetHttpRequestData(): Performance gotcha
time: 2020-12-07 14:10:00 +00:00
---

[GetHTTPRequestData()](https://docs.lucee.org/reference/functions/gethttprequestdata.html) is a built in CFML function that retrieves:

* HTTP Headers
* HTTP Method
* HTTP Body

Handy! However, the **HTTP Body** part has issues. Ben Nadel blogged about one such issue here: [https://www.bennadel.com/blog/2824-gethttprequestdata-may-break-your-request-in-coldfusion-but-gethttprequestdata-false-may-not.htm](https://www.bennadel.com/blog/2824-gethttprequestdata-may-break-your-request-in-coldfusion-but-gethttprequestdata-false-may-not.htm).

I discovered another issue today. Each time you call `GetHttpRequestData()`, Lucee will re-read the input stream of the request to populate the body, consuming memory in the process.

This won't necessarily show up with some cursory testing of request _times_. However, if you use something like FusionReactor to test the amount of memory a request burns through you are likely to see some unexpectedly large requests when:

* The request body is large (i.e. file uploads, big API payloads)
* You call `GetHttpRequestData()` multiple times.

The solution, as Ben points out, is to use `GetHttpRequestData( false )` if you do not care about the request body.