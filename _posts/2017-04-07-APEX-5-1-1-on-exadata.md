---
layout: post
title: Oracle Application Express 5.1.1 on Exadata cluster
---

Our DBA team really likes using APEX to develop DBA applications. We are a pretty small team and we like to automate and standardize everything we do. We make a habit of using database tables to store our configuration information and we write scripts that read those DBA tables in order to support our business users.

We recently took control of a new Exadata cluster and of course we created a DBA repository database as we were creating the business databases. Since APEX 5.1.1 was just released, we decided to replace our old APEX system. That system was was a single instance running on a virtual machine and controlled by an outsourced administration team. We are a lot more comfortable having control over installation and maintenance of a system we depend on to get our work done.

The new Exadata is a four node RAC and our new DBA repository has an instance running on each node. We did not want to install software on the Exadata so that ruled out using Rest Data Services (aka APEX Listener). It also ruled out using Oracle HTTP Server with mod_plsql.

Since this APEX system is only used by our DBA team and is not exposed to outside networks we felt comfortable using the Embedded PL/SQL Gateway. However, the Oracle documentation contains this ominous note: "Oracle recommends that you do not select the Embedded PL/SQL Gateway option for Oracle RAC installations. Because the Embedded PL/SQL Gateway uses an HTTP Server built into the database instance, it does not take advantage of the Oracle RAC shared architecture." (https://docs.oracle.com/database/apex-5.1/HTMIG/choosing-web-listener.htm#HTMIG370)

What to do? We decided to move forward with the Embedded PL/SQL Gateway and see how useable it would be. We could always spin up another virtual machine later if we needed to go the Rest Data Services route. I went through the steps in the APEX documentation to set up the Embedded PL/SQL Gateway. I could not get a response when I hit the database machine APEX URL that I had configured. I poked around on the servers and noticed that the XML db listeners were not running. I had forgotten that when I created the DBA database I intentionally did not install any options like Java and XML db. After installing XML db and restarting the database APEX started working. (https://docs.oracle.com/database/121/ADXDB/appaman.htm#ADXDB2700)

But that's not all - here's the real reason I wanted to blog about this. This APEX system is reachable using the scan listener! So that scary quote in the documentation should be ignored. We can connect using a URL like this: http://corpexa-scan.corp.com:8080/apex . No server names are required. So we CAN take advantage of RAC functionality.

This is just what we wanted so we are very satisfied with our new APEX system.



