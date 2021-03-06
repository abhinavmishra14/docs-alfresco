---
author: [Alfresco Documentation, Alfresco Documentation]
audience: 
---

# Installing Alfresco on Tomcat

For more complex Alfresco installations or if you wish to use an existing Tomcat application server, you can use the Web Archive \(WAR\) bundle to install Alfresco on any platform. For this type of installation, you must ensure that the required software is installed on the machine.

Use this method of installing Alfresco if you already have installed a JRE, a supported database, an application server, and the additional Alfresco components.

For information about securing Tomcat, see [Tomcat security considerations](https://tomcat.apache.org/tomcat-7.0-doc/security-howto.html).

-   **[Installing the Tomcat application server](../tasks/configfiles-change-path.md)**  
Install an instance of Tomcat 7 manually and modify it to use the correct directory structure and files for Alfresco.
-   **[Installing the Alfresco WARs](../tasks/alf-war-install.md)**  
A WAR file is a JAR file used to distribute a collection of files \(JavaServer Pages, servlets, Java classes, XML files, tag libraries, and static web pages\) that together constitute a web application.
-   **[Installing Alfresco into an existing web application](../tasks/install-server-root.md)**  
If you install Alfresco manually, you must deploy the `ROOT.war` application to the server root. If you already have an application running in the server root, you can merge the Alfresco function into your existing web application.
-   **[Configuring Alfresco as a Windows service](../tasks/alf-winservice.md)**  
To configure Alfresco to run as a Windows service, you need to set up the application server \(Tomcat\) to run as a Windows service.

**Parent topic:**[Installing Alfresco manually](../concepts/ch-install.md)

