---
layout: default
title: Flag 4
nav_order: 10
parent: Project Log4Shell!
grand_parent: Projects
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
