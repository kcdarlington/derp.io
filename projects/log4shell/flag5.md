---
layout: default
title: Flag 5
nav_order: 12
parent: Project Log4Shell!
grand_parent: Projects
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
  
