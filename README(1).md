Using CDI Asynchronous Events    
===============================

 [Description](#description)   [Build & Configure](#build)   [Run](#run)   [Troubleshoot](#troubleshooting)   [Related Topics](#related_topics)  [Start to Run](/cdi20-async-event)

About the Example
-----------------

The CDI Events feature allows beans to interact in a decoupled fashion with no compile-time dependencies between the interacting beans. It follows the Observer pattern. Event producers raise events that are consumed by observers. The event object carries state from producer to consumer. The producer can fire the events and the observer can specify qualifiers to narrow the set of events received.

Events may also be fired asynchronously using one of the methods `fireAsync()`. Event fired with the `fireAsync()` method is fired asynchronously. All the resolved asynchronous observers are called in one or more different threads. Method `fireAsync()` returns immediately.

* * *

Where is my [heading](#MyHeading)?

Files Used in the Example
-------------------------

Directory Location:

@EXAMPLES\_HOME/samples/wls.15.1.1/cdi/async-events/

File

Click source files to view code.

Description

[pom.xml](pom.xml)

Maven build file that contains targets for building and running the example.

[WebController.java](src/main/java/examples/cdi/cdi20/asyncevents/WebController.html)

A JSF managed bean that invokes the AsyncEventProducer start method to fire CDI events.

[AsyncEventProducer.java](src/main/java/examples/cdi/cdi20/asyncevents/AsyncEventProducer.html)

Fires asynchronous events.

[AsyncEventObserver.java](src/main/java/examples/cdi/cdi20/asyncevents/AsyncEventObserver.html)

Consume events.

[index.xhtml](src/main/webapp/index.html)

Index page for this example, which describes this feature and example, and verifies the functions.

[web.xml](src/main/webapp/WEB-INF/web.xml)

Web application deployment descriptor.

[beans.xml](src/main/webapp/WEB-INF/beans.xml)

CDI deployment descriptor file to enable CDI features.

* * *

Description
-----------

This sample demonstrates how to produce asynchronous events and how singleton EJBs can consume these events.

The `AsyncEventProducer` fires asynchronous events:

@Inject
private Event event;

public void start() {
  for (int i = 0; i < EVENT\_NUMBER; i++) {
     try {
        Thread.sleep(1000);
     } catch (InterruptedException e) {
        LOGGER.log(Level.SEVERE, "InterruptedException Exception" + e.getMessage());
     }
     event.fireAsync(new Integer(i));
     LOGGER.log(Level.INFO,"Fired CDI event-" + i + " from thread "+ Thread.currentThread().getName());
  }
}

The `AsyncEventObserver` consumes asynchronous events:

public void onMessage(@ObservesAsync @Default Integer I) {
   LOGGER.log(Level.INFO, "Async event-" + I + " in thread "+ Thread.currentThread().getName());
}

* * *

Building and Configuring the Example
------------------------------------

### Prerequisites

Before working with this example:

1.  [Install WebLogic Server](@DOCSWEBROOT/wlsig/index.html), including the examples.
2.  [Start the Examples server.](javascript:reloadTOC('../../../examples.html#startServer'))
3.  [Set up your environment](javascript:reloadTOC('../../../examples.html#environment')).

### Build and Deploy the Example

This sample is already built and deployed in the `wl_server` WebLogic Server Examples domain, so you can skip these steps and proceed directly to running the example.  

1.  Change to the `@EXAMPLES_HOME/samples/wls.15.1.1/cdi/async-events` directory.
2.  Execute the following command:  
      
    `mvn clean package`  
      
    to compile and stage the example.
3.  Execute the following command:  
      
    `mvn com.oracle.weblogic:weblogic-maven-plugin:deploy -Dsource=target/cdiAsyncEvents-1.0.0-SNAPSHOT.war -Duser=<user> -Dpassword=<password> -Dadminurl=t3s://127.0.0.1:7002`  
      
    to deploy the example in the WebLogic Server Examples domain.  
      
    

* * *

Running the Examples
--------------------

1.  Complete the steps in the "[Building and Configuring the Example](#build)" section.
2.  Execute the following command:  
      
    `ant run`  
      
    which opens your default browser to display this example application home page. Click the **Start** button, then check the console outputs. You should get some messages that are similar to the following ones.

    Apr 11, 2018 3:50:02 PM examples.cdi.cdi20.asyncevents.AsyncEventProducer start
    INFO: Fired CDI event-0 from thread \[ACTIVE\] ExecuteThread: '7' for queue: 'weblogic.kernel.Default (self-tuning)'
    Apr 11, 2018 3:50:02 PM examples.cdi.cdi20.asyncevents.AsyncEventObserver onMessage
    INFO: Async event-0 in thread weld-worker-1
    Apr 11, 2018 3:50:03 PM examples.cdi.cdi20.asyncevents.AsyncEventProducer start
    INFO: Fired CDI event-1 from thread \[ACTIVE\] ExecuteThread: '7' for queue: 'weblogic.kernel.Default (self-tuning)'
    Apr 11, 2018 3:50:03 PM examples.cdi.cdi20.asyncevents.AsyncEventObserver onMessage
    INFO: Async event-1 in thread weld-worker-2
    Apr 11, 2018 3:50:04 PM examples.cdi.cdi20.asyncevents.AsyncEventProducer start
    INFO: Fired CDI event-2 from thread \[ACTIVE\] ExecuteThread: '7' for queue: 'weblogic.kernel.Default (self-tuning)'
    Apr 11, 2018 3:50:04 PM examples.cdi.cdi20.asyncevents.AsyncEventObserver onMessage
    INFO: Async event-2 in thread weld-worker-3
    Apr 11, 2018 3:50:05 PM examples.cdi.cdi20.asyncevents.AsyncEventProducer start
    INFO: Fired CDI event-3 from thread \[ACTIVE\] ExecuteThread: '7' for queue: 'weblogic.kernel.Default (self-tuning)'
    Apr 11, 2018 3:50:05 PM examples.cdi.cdi20.asyncevents.AsyncEventObserver onMessage
    INFO: Async event-3 in thread weld-worker-4
    Apr 11, 2018 3:50:06 PM examples.cdi.cdi20.asyncevents.AsyncEventProducer start
    INFO: Fired CDI event-4 from thread \[ACTIVE\] ExecuteThread: '7' for queue: 'weblogic.kernel.Default (self-tuning)'
    Apr 11, 2018 3:50:06 PM examples.cdi.cdi20.asyncevents.AsyncEventObserver onMessage
    INFO: Async event-4 in thread weld-worker-1
    Apr 11, 2018 3:50:07 PM examples.cdi.cdi20.asyncevents.AsyncEventProducer start
    INFO: Fired CDI event-5 from thread \[ACTIVE\] ExecuteThread: '7' for queue: 'weblogic.kernel.Default (self-tuning)'
    Apr 11, 2018 3:50:07 PM examples.cdi.cdi20.asyncevents.AsyncEventObserver onMessage
    INFO: Async event-5 in thread weld-worker-2
    ...
  

Troubleshooting
---------------

*   [General Troubleshooting Tips for Examples](javascript:reloadTOC('../../../examples.html#troubleshooting'))

* * *

Related Topics
--------------

*   [Developing Web Applications, Servlets, and JSPs for Oracle WebLogic Server](@DOCSWEBROOT/wbapp/index.html)
*   [Developing Applications for Oracle WebLogic Server](@DOCSWEBROOT/wlprg/index.html)

* * *

[Copyright](../../../copyright.html) © 2025 Oracle and/or its affiliates. All rights reserved.
