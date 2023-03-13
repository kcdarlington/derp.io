---
layout: default
title: Intro
nav_order: 6
parent: Project Log4Shell!
grand_parent: Projects
---

<div style="text-align:center">
  <span id="intro" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">INTRO</span>
</div>
Now that we are set up, let's do a quick rundown of Log4j, how it works at a high level, and test that we are able to successfully call and exploit the application. 

As you should know from the background and resource’s section, Log4j is an application logging library that outputs user defined program information. An example log statement would look like the following:

```java
  static Logger log = LogManager.getLogger(RestServlet.class.getName());
  log.debug("ApplicationId: {}", applicationId);
```

The log statement above as you can see, defines the class, log level of the message and the actual message. Notice that we are injecting user input into the log message. This is where our vulnerability is. 

To be more specific, here is what the output of the message actually will look like:
![alt text](/images/logmsg1.PNG)
  
Now we can map the code to the logged message. The date/time is defined in configuration files which are out of the scope of this project. Next is where our code starts to map. We have [Classname.java:LineNumber]. This is helpful to know exactly what part of your code is executing and where. 

Next, we see DEBUG. This is what is called the log level. Log levels are used for the amount of output you want your application to log, or even to classify errors different from informative messages. The typical log levels are DEBUG, INFO, WARN, ERROR, FATAL. To further explain, if a log level is set to say ERROR, your application will not log anything on WARN, INFO, or DEBUG level. This application is set to DEBUG.

Finally, we get to the actual logged message. As you can see, we log the constant text as well as the injected variable applicationId, which is our applicationId. This is the most important part you will want to pay attention to. Throughout this project, you will try to find messages that log user defined input and inject your malicious string into it. 

Now, let's have some fun and get familiar with the application. To do so, we will call the application normally and then lookup the java version on the application server.

To start we can run a simple inquiry to the services /ping endpoint and see what we get back from the logs and see if we can find anything that is exploitable.

<span style="color: #003057; font-size:25px; font-weight: bold;">*****************GATECH_ID IS A REQUIRED HEADER*************** </span> \
<span style="color: #003057; font-size:25px; font-weight: bold;">NOTE: This is not the Georgia Tech Username, it is the gtId that you can find on your Buzzcard or here: [GatechId](https://public.webapps.gatech.edu/cfeis/gtid/).</span>

Open a new terminal and run:
```bash 
  curl 'http://localhost:8080/rest/users/ping' -H 'GATECH_ID:123456789'
```

You should see your logs log some messages in the log tailing terminal window. Let's inspect it.

Go back to the terminal window you are tailing the logs in. When the server intercepts a request, it logs “Request intercepted” to alert the user where the request starts. As we can see, there is not much going on in terms of useful information, but we can see the service did log a message that could be exploitable. 

Voila! In the highlighted message, we see that it is logging the Method Type: GET, the URL: /rest/users/ping, and some headers that are null. This is a good indicator that we can exploit this service by sending lookups through a header. 
![alt text](/images/introrequest.PNG)

Let's call one more endpoint to see how headers work and what the application does when we request user data. 

In your curl terminal, run the following command: 
```bash
curl 'http://localhost:8080/rest/users/userlist' -H 'GATECH_ID:123456789' -H 'Accept:application/json' -H 'X-UserName:rcoleman8'
```
Your output should be similar to:
![alt text](/images/firstrequest.PNG)
  
Take some time to inspect the logged messages and try to understand what the program is doing and the flow of it. 

Lets try to get the java version on the server now.

The checked headers for this application are content-type, accept, and X-UserName.

Construct a malicious payload using one of the logged headers that will return the java version of the host of the web application. You should see something like the screenshot below if successful:
![alt text](/images/loggedheader.PNG)
  
Do you see the same output? LIGHTWEIGHT BABY! Muahaha! If not, try to research the log4shell exploit more and learn how to exploit the lookup.

Our hunch was correct and we have successfully exploited the vulnerability. You can play around with this if you like and see what other lookups you can perform. It is possible to lookup system settings, environment variables and much more just with this. 

<mark><span style="color: #003057; font-size:25px; font-weight: bold; text-decoration:underline">Be sure to save your work outside of the VM in case the VM crashes or some other unforeseen issue arises. This will ensure you are not losing your work.</span></mark>
