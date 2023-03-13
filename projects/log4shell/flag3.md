---
layout: default
title: Flag 3
nav_order: 10
parent: Project Log4Shell!
grand_parent: Projects
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
