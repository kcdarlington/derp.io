---
layout: default
title: Flag 2
nav_order: 8
parent: Project Log4Shell!
grand_parent: Projects
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
