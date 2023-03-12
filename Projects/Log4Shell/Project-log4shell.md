---
layout: default
title: Project Log4Shell!
nav_order: 1
#has_children: true
parent: Project Log4Shell Home
---

<div style="text-align:center">
  <span style="color: #003057; font-size:36px; font-weight: bold">Project</span> <span style="color:#eeb211; font-size:36px; font-weight: bold"> Log4Shell</span><span style="color: #003057; font-size:36px; font-weight: bold">!</span>
</div>

<span style="color: #003057; font-size:24px; font-weight: bold;">Learning Goals of this Project:</span>

Students will learn about a real world critical Java exploit Log4Shell (https://nvd.nist.gov/vuln/detail/CVE-2021-44228 and https://www.randori.com/blog/cve-2021-44228/). This lab develops an understanding of sending malicious Java payloads via jndi/ldap lookups to exploit a vulnerable java application that uses a Log4j logger. A simple LDAP library will be used to assist students complete this lab.

<span style="color: #f3172d; font-weight: bold; font-size:18px; text-decoration:underline">THIS IS A REAL WORLD CRITICAL VULNERABILITY THAT MOST VENDORS HAVE PATCHED BUT THERE STILL COULD BE APPLICATIONS WITHOUT THE PATCH. THIS PROJECT IS FOR EDUCATIONAL PURPOSES ONLY. ATTEMPTING THIS ON REAL APPLICATIONS COULD PUT YOU IN VIOLATION OF THE LAW AND GEORGIA TECH IS NOT RESPONSIBLE.

<span style="color: #003057; font-size:24px; font-weight: bold;">The final deliverables:

A single JSON formatted file will be submitted to Gradescope. This file should be named project_log4shell.json. A template can be found in the Home directory.
See <a href="#submission">Submission Details</a> for more information.

<span style="color: #003057; font-size:24px; font-weight: bold;">Important Reference Material:

* [Project Introduction](https://www.youtube.com/watch?v=cmnUOYkI6A4) (Might not match the project requirements exactly)
* This simple  [LDAP server](https://github.com/mbechler/marshalsec)  that will be used to run the exploit.
* This [Log4JExploit Intro](https://www.lunasec.io/docs/blog/log4j-zero-day/) and [How Log4Shell Works](https://news.sophos.com/en-us/2021/12/17/inside-the-code-how-the-log4shell-exploit-works/) to familiarize yourself with the exploit and how it is accomplished.
* If you have no experience in Java, Log4j/logging, RESTful applications, JNDI, LDAP, we STRONGLY encourage you to do research into the topics. There are many great resources online like Google and YouTube. 
* [Log4J Documentation](https://logging.apache.org/log4j/2.x/)
* [Hands on Introduction to Log4Shell exploit in general (not this project but helpful)](https://www.youtube.com/watch?v=lJeAgQQaDEw)
* [Helpful Linux Networking Commands](https://javarevisited.blogspot.com/2010/10/basic-networking-commands-in-linuxunix.html)
* [NCAT Command](https://www.linuxtechi.com/nc-ncat-command-examples-linux-systems/)
* [Log4Shell Example](https://securityblue.team/log4j-hunting-and-indicators/)
* If you would like to learn more about this exploit and Java Object Deserialization Vulnerabilities this paper written by Moritz Bechler is an excellent research paper: [Java Unmarshaller Security](https://github.com/mbechler/marshalsec/blob/master/marshalsec.pdf) and so is this BlackHat presentation: [A JOURNEY FROM JNDI/LDAP MANIPULATION TO REMOTE CODE EXECUTION DREAM LAND](https://www.blackhat.com/docs/us-16/materials/us-16-Munoz-A-Journey-From-JNDI-LDAP-Manipulation-To-RCE.pdf)

<span style="color: #003057; font-size:24px; font-weight: bold;">Submission:    
    <span style="color: black; font-size:18px; font-weight: bold;">Gradescope (autograded) - see <a href="#submission">Submission Details</a>

<span style="color: #003057; font-size:24px; font-weight: bold;">Virtual Machine:
<ul>(Note: downloads can be very slow when project first releases due to very high traffic in first few hours/day) </ul>

* Apple M1 based systems
  * You cannot complete this project on an M1 based system.

* Intel/AMD x64 version
    * [Windows Virtualbox 6.1.16 Download](https://download.virtualbox.org/virtualbox/6.1.16/VirtualBox-6.1.16-140961-Win.exe)
    * [Mac VirtualBox 6.1.16 Download (for Intel Macs only)](https://download.virtualbox.org/virtualbox/6.1.16/VirtualBox-6.1.16-140961-OSX.dmg)
    * Same as Project Capture the flag or [VM Download](https://cs6035.s3.amazonaws.com/CS6035-Student-Spring-2023-v7.ova)
    * Username: log4j, Password: konami-code. You will need to log out of the user for the previous CTF project and log in to the log4j user.

  
---
  
  
<div style="text-align:center">
  <span id="setup" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">SETUP</span>
</div>
  
To get setup for the flags, follow the steps carefully below, and be sure you are running each in a separate terminal window as noted. 

You will need switch users to login to log4j user via:
 
```
  Username: log4j  Password: konami-code
```

In the home directory of log4j user, start the container with the start script: 
```bash 
  sudo ./StartContainer.sh
```

Open a new terminal window and go to “Desktop/log4shell/logs”:  
```bash 
  cd Desktop/log4shell/logs
```

Run the following command to view the logs:
```bash 
  tail -f cs6035.log
```

You should now see the tail of the log file from the application running. 

![alt text](/images/logoutput.PNG)

  *****If the logs stop populating, then just stop and restart the tail. This is happening because the data logged gets too large so the log “rolls over” to another file.*******


<span style="font-weight: bold">1. Run the LDAP Server:</span> \
Open a new terminal window and run the following command to set the current directory to “Desktop/log4shell/target”:  
```bash
  cd ~Desktop/log4shell/target
```
Next, start the LDAP server by running:
```bash 
  java -cp marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://127.0.0.1:4242/#Exploit"
```
You can get the ip address of the vm by running the block below in a terminal
```bash 
  ip addr show
``` 
This outputs the vm’s \<IP:PORT\>
```bash 
  127.0.0.1:4242
```
**It is very important that this matches the port specified in the Malicious server. If your exploit is not working because it is not connecting to the malicious server, your ports likely do not match OR the vm’s IP is not correct.**
You should see the following output:
![alt text](/images/ldapoutput.PNG) 
  
<span style="font-weight: bold">2. Run the Malicious Server:</span> \
Open a new terminal and make sure the active directory is the directory that contains your malicious .class file. For simplicity, we have created “Desktop/log4shell/{flag_no}” for you to work in. <span style="font-weight: bold">Do not leave this directory</span>. Run the server in “Desktop/log4shell/{flag_no}” by the following command:
```bash 
  python3 -m http.server 4242 
``` 
**It is very important that this matches the port specified in the LDAP server. If your exploit is not working because it is not connecting to the malicious server, your ports likely do not match OR the vm’s IP is not correct**
You should see the following output:
![alt text](/images/pythonserveroutput.PNG) 
  
<span style="font-weight: bold">3. Read data that is flowing on the network (This step is required for Flag 2 but is optional for the rest):</span> \
Open a terminal and run:
```bash 
  nc -nlvp <your_desired_port> 
``` 
You should see the following output:
![alt text](/images/nlvp.PNG)

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
  
---
  
<div style="text-align:center">
  <span id="flag1" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">FLAG 1: Environment Echo (12.5 pts)</span>
</div>  

Make sure you have went through the <a href="#setup">Setup</a> and <a href="#intro">Intro</a> sections.
  
If you haven’t already, run the start script in the home directory of log4j user, start the container with the start script: 
```bash
  sudo ./startContainer.sh
```

The endpoint for this exploit can be called and inspected via:
```bash
curl 'http://localhost:8080/rest/users/ping' -H 'GATECH_ID:123456789' -H 'Accept:application/json'
```

Now that you have seen how to check environment variables, run a lookup for ADMIN_PASSWORD which stores your flag for this exercise. If successful, you should see the below output:
![alt text](/images/flag1.PNG)
  
Add this flag (Congratulations! Your flag1 is: __________) to your project_log4shell.json file.
  
---
  
<div style="text-align:center">
  <span id="flag2" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">FLAG 2: Get a Shell (12.5 pts)</span></div>

Make sure you have went through the <a href="#setup">Setup</a> and <a href="#intro">Intro</a> sections. 
  
If you haven’t already, run the start script in the home directory of log4j user, start the container with the start script: 
```bash
  sudo ./startContainer.sh
```
  
The endpoint for this exploit can be called and inspected via:
```bash
curl 'http://localhost:8080/rest/users/ping' -H 'GATECH_ID:123456789' -H 'Accept:application/json'
```

Now that we have proven this service is vulnerable and that we can exploit it in at least one place, let's try to do more damage and do something more malicious. If you have not already, you NEED to read through the suggested readings and learn how/why it is possible to send jndi lookups.

Open the “Exploit.java” file and construct a malicious payload to execute such that when the jndi/ldap lookup happens, it gives you root access on the vulnerable application server.

You should have a total of 4 terminal windows open which are the 3 from the SETUP section and one terminal window to run your curl command.

Once you are ready to run the exploit, ensure that the java version you are running the command is “java version 1.8.0_20” by running:
```bash
  java -version
```
![alt text](/images/javav.PNG)  

To compile your .java file into a class file, move the the directory the .java file is stored in and run:
```bash
  javac Exploit.java
```

We have added the ability to log from your Exploit.java file so that you can log useful debugging information while you are developing your exploit. To use the logger, uncomment the logging code (And imports) and compile using the following command:
```bash
  javac -cp "../apache-log4j-2.9.1-bin/*" Exploit.java
```
  
Make sure you are running this command from ```Desktop/log4shell/Flagx```
![alt text](/images/javac.PNG)    
  
This should create Exploit.class. <mark><span style="font-weight: bold">Run your "python3 -m http.server 4242" command in this directory.</span></mark>

<span style="font-weight: bold">NOTE: THIS PROJECT IS WRITTEN USING THE VULNERABLE VERSION OF 1.8.0_20. NOT ALL VERSIONS OF JAVA ALLOW LOOKUPS AND YOU COULD SPIN YOUR WHEELS FOR AWHILE NOT KNOWING WHY YOUR EXPLOIT IS NOT RUNNING IF YOU DO NOT FOLLOW THIS DIRECTION.</span>

<span style="font-weight: bold">Hint: Pay close attention to the ports you are using in this exercise.</span>

If successful, you should see similar console output as the screenshot below:
![alt text](/images/allflag2.PNG) 
  
With success, you should see this in the console output of the nc command. Now type “whoami” and you should see “root”.
![alt text](/images/whoami.PNG) 
  
Did you get the screenshot above? Congratulations!

If not, keep trying and ensure all of your hosts/ports match. Analyze the screenshots and make sure yours matches. If you are not getting the ldap/python server output, you likely did not set up your hosts/ports correctly OR your Exploit.java file isn’t correct. 

You now have root access to the vulnerable application’s server. YIKES! This is a great example why this exploit is so dangerous. You can perform any task on this server now. For now, go back 2 directories using “cd ..” and then run “java -jar Flag2.jar” to get your flag and add it to the json under “Flag2”.
![alt text](/images/flag2success.PNG) 

---  
  
<div style="text-align:center">
  <span id="flag3" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">FLAG 3: Bypass Authentication (15 pts)</span>
</div> 
  
Make sure you have went through the <a href="#setup">Setup</a> and <a href="#intro">Intro</a> sections. 

If you haven’t already, run the start script in the home directory of log4j user, start the container with the start script: 
```bash
  sudo ./startContainer.sh
```

Now that you have gotten familiar with this environment and exploit, let's try for another, more involved flag. We will try exploiting another URI:

``` http://localhost:8080/rest/users/userlist ```

This endpoint will deliver a list of valid users a program can choose from.  It uses a very weak form of authentication that verifies a user is permitted by validating their user id that they send through the “X-UserName” header field.

The goal here is to use your gatech id number in the  “X-UserName” header (in addition to the GATECH_ID header) and have the application validate successfully.
Example:
  
``` -H 'X-UserName:123456789' ```

First Run:
```bash
  curl 'http://localhost:8080/rest/users/userlist' -H 'GATECH_ID:123456789' -H 'Accept:application/json' -H   'X-UserName:rcoleman8'
```

and inspect the logged output. See if you can find anything that gives you a hint about how to exploit it/what you need to do to bypass the authentication. 

Update Exploit.java to send a malicious payload that will bypass the endpoint’s validation by sending your gatech id in the ‘X-Username’ header. How you will bypass authentication can be found by analyzing the applications logged output. Follow the steps in the SETUP/INTRO section to run your exploit. 

Upon success, you will see the output below, where the blacked out AuthorizedUsers is your UserId and your flag is right below it..
![alt text](/images/flag3.PNG) 
  
<span style="font-weight: bold">Hint: Someone might have tried to roll their own patch and tried to deny requests containing malicious string patterns.</span> \
<span style="font-weight: bold">*********IF THIS FLAG COMES OUT BLANK, Restart container by running the stopContainer and startContainer scripts in the home directory of the log4j user.***********</span>
  
---  
  
<div style="text-align:center">
  <span id="flag4" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">FLAG 4: Bypass Authentication (15 pts)</span>
</div> 
  
Make sure you have went through the <a href="#setup">Setup</a> and <a href="#intro">Intro</a> sections. 

If you haven’t already, run the start script in the home directory of log4j user, start the container with the start script: 
```bash
  sudo ./startContainer.sh
```

You have caught wind that this service caches database authorized users’ data in a file on the file system. Other applications use this service to get users’ data to cache and allow anyone who is an authorized user to access them. You understand how terrible of an idea this is and realize you are going to make them pay for dumb ideas. 

For this flag, you are going to bypass authentication (run Flag 3 again) and then UPDATE the <span style="font-weight: bold">UsersList.txt</span> on the server to add your GaTech student Id to replace one of the users’ names. Do not delete any of the users as the service checks to make sure that they are all there and you do not want to get caught. You should also keep the user’s id you overwrite intact. For example, if you decide you want to overwrite user(1,"Tim Cook","CEO"), it should be updated to  user(1,"your_gatech_id","CEO"). 

If you haven’t already, run the start script in the home directory of log4j user, start the container with the start script: 
```bash
  sudo ./startContainer.sh
```

Run: 
```bash 
  curl 'http://localhost:8080/rest/users/user/?userId=1' -H 'GATECH_ID:123456789' -H 'X-UserName:rcoleman8' 
```
  
and inspect the logged output. See if you can find anything that gives you a hint about how to exploit it/what you need to do to bypass the authentication. 

If successful, your flag will be stored in the response value object’s name like the screenshot below:
![alt text](/images/flag4.PNG)   
  
<span style="font-weight: bold">Hint: You might have to run more than one LDAP and have more than one Exploit file.</span> \
<span style="font-weight: bold">*********IF THIS FLAG COMES OUT BLANK, Restart container by running the stopContainer and startContainer scripts in the home directory of the log4j user.***********</span>
  
---  
  
<div style="text-align:center">
  <span id="flag5" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">FLAG 5: Command and Concat (20 pts)</span>
</div>   
 
Make sure you have went through the <a href="#setup">Setup</a> and <a href="#intro">Intro</a> sections. 

If you haven’t already, run the start script in the home directory of log4j user, start the container with the start script: 
```bash
  sudo ./startContainer.sh
```

The endpoint for this exploit can be called and inspected via:
```bash
  curl 'http://localhost:8080/rest/users/user/updateuser' -H 'GATECH_ID:123456789' -H 'Content-  Type:application/json' -H 'X-UserName:rcoleman8' --data-raw '{"id": 1,"name": "User Name","profession":   "User Profession"}'
```

Your output should look like this:
![alt text](/images/flag5output.PNG)   
  
Take some time to inspect this output and see what you need to exploit.Yes the exception is expected and maybe even a clue above it ;)

For this flag, you will construct a malicious file (Exploit.java) and compile it, so that when deserialized it will create a simple “.txt” file on the server to get the flag. Using the log4shell exploit, create a file named “Flag5.txt” on the server and add ONE line that says “LightWeightBaby!”. If you do not follow this exactly, you will not get this flag.

Upon success, you will see the output below. (You might have to scroll for this)
![alt text](/images/flag5.PNG)   
  
---  
  
<div style="text-align:center">
  <span id="flag6" style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">FLAG 6: Command and Concat (20 pts)</span>
</div>  
    
Make sure you have went through the <a href="#setup">Setup</a> and <a href="#intro">Intro</a> sections. 

If you haven’t already, run the start script in the home directory of log4j user, start the container with the start script: 
```bash
  sudo ./startContainer.sh
```
  
For this flag, we will exploit a previous endpoint that publishes updates to a topic on the server. 
```bash 
  curl 'http://localhost:8080/rest/users/user/updateuser' -H 'GATECH_ID:123456789' -H 'Content-Type:application/json' -H 'X-UserName:rcoleman8' --data-raw '{"id": 1,"name": "User Name","profession": "User Profession"}'
```

You have caught wind that the service reads from a properties file. 

For this exploit, you will use the log4j exploit to overwrite the “config.properties” file saved in the root directory of the application. This properties file contains a topic that the application will publish a message to when updateUser call is made (the application is also subscribed to this topic as you can see in the logs). 

You will need to trick the application into publishing a message to a different topic with your GATECH_ID as the account number in order to generate a valid flag. 

Upon success, you should see your output similar to that below:
![alt text](/images/flag6.PNG) 

<span style="font-weight: bold;">Hint: Look through the cs6035.log to find clues about what this other topic could be.</span> 
	
*<span style="font-weight: bold;">Your flag could be invalid if you have not sent your GATECH_ID appropriately in the published message.</span>*
	
---
	
<span id="submission" style="color: #003057; font-size:24px; font-weight: bold;">Submission Details:</span> 
	
<span style="font-weight: bold; text-decoration:underline">File submission instructions:</span>
  
This project needs to be submitted via gradescope. Navigate to the course in Canvas, click ‘Gradescope’, click ‘Project Log4Shell’ and submit there. 

The contents of the submission file should be the following. There is a ~/project_log4shell.json file in your vm with a template set up, or you can copy-paste this to your newly created <span style="font-weight: bold;">project_log4shell.json</span> file elsewhere and replace the placeholders with the flags you retrieve from each relevant task.

*Note: You can use TextEdit or Vim to create and edit this file. Do not use LibreOffice or any Word Document editor. It must be in proper JSON format with no special characters in order to pass the autograder and these Word Document editors are likely to introduce special characters.*

If you can’t find the file in the VM just copy this format below:
```json
{
	"flag1": "<copy flag 1 here>",
	"flag2": "<copy flag 2 here>",
	"flag3": "<copy flag 3 here>",
	"flag4": "<copy flag 4 here>",
	"flag5": "<copy flag 5 here>",
        "flag6": "<copy flag 6 here>"
}
```

An example of what the submitted file content should look like:
```json
{
 "flag1": "4ec60c3e084d8387f0f33916e9b08b99d5264a486c29130dd4a5a530b958c5c0f1faeaca2ce30b478281ec546a4729f629b531a86cb27d86c089f0c542",
 "flag2": "f496d9514c01e8019cd2bc21edfeb8e33f4a29af14a8bf92f7b3c14b5e06c5c0f1faeaca2ce30b478281ec546a4729f629b531a86cb27d86c089f0c442",
 "flag3": "b621bba0bb535f2f7a222bd32994d3875bcfcad651160c543de0a01dbe2e0c5c0f1faeaca2ce30b478281ec546a4729f629b531a86cb27d86cf0c49542",
………………………………………………………………………………………
}
```
