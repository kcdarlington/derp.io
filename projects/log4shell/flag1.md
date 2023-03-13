---
layout: default
title: Flag 1
nav_order: 8
parent: Project Log4Shell!
grand_parent: Projects
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
 
