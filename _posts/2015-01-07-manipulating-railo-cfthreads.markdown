---
layout: post
title: Manipulating Railo's cfthreads
time: 2015-01-07 13:25:00 +00:00
---

I had a need to start some threads in the background and then be able to monitor and potentially terminate them from another request. Digging around, I couldn't find any way to do this natively and so came up with this solution that uses native java threads. 

*Disclaimer: there may well be a way to do this natively, or perhaps I shouldn't be using cfthread for this type of task at all, but the following code works all the same.*

<!--more-->

## Get the actual running threads on the JVM

This is really straightforward thankfully with a single line to get an array of Java Thread objects:

{% highlight js %}
array function getJavaThreads() output=false {
  return CreateObject( "java", "java.lang.Thread" ).getAllStackTraces().keySet().toArray();
}
{% endhighlight %}

What we then want to do is filter those threads to only show those that have been started with cfthread. My somewhat hacky solution to this looks like this:

{% highlight js %}
struct function getRunningCfThreads() output=false {
  var javaThreads = getJavaThreads();
  var cfthreads   = {};

  for( var thread in javaThreads ) {
    if ( thread.getName() contains "cfthread" ) {
      try {
        var cfThreadScope = thread.getThreadScope();
        cfthreads[ cfThreadScope.name ] = cfThreadScope;
      } catch( any e ) {}
    }
  }

  return cfthreads;
}
{% endhighlight %}

From here, you can access all the usual cfthread attributes as per the documentation, i.e. `ELAPSEDTIME`, `NAME`, `OUTPUT`, `PRIORITY`, `STARTTIME`, `STATUS` and `STACKTRACE`:

{% highlight js %}
var threads        = getRunningCfThreads();
var myThreadStatus = threads[ myThreadName ].status      ?: "";
var elapsedTime    = threads[ myThreadName ].elapsedTime ?: "";

// etc.
{% endhighlight %}

## Terminating a thread

To terminate a thread, we can use the `interrupt()` method on the java thread object. So, given a cfthread name and nothing else, we could do something like:

{% highlight js %}
void function terminateCfThread( required string threadName ) output=false {
  var javaThreads = getJavaThreads();

  for( var thread in javaThreads ) {
    if ( thread.getName() contains "cfthread" ) {
      try {
        var cfThreadScope = thread.getThreadScope();
        if ( cfThreadScope.name == arguments.threadName ) {
          thread.interrupt();
          return;          
        }
      } catch( any e ) {}
    }
  }
}
{% endhighlight %}

## Conclusion

This is very rough code that I just wanted to put out there. If it turns out that this is useful and I'm not missing some obvious point, I'll work it into some cleaner code and create a utility on GitHub or somesuch. Would be good to hear your thoughts.

Dominic