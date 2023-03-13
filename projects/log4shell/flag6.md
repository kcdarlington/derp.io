---
layout: default
title: Flag 6
nav_order: 12
parent: Project Log4Shell!
grand_parent: Projects
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
