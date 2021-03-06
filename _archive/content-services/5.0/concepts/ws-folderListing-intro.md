---
author: Alfresco Documentation
---

# Developing a Folder Listing web script

This tutorial describes how to create a Folder Listing web script that mimics the behaviour of the `dir` command in Microsoft Windows, or `ls` in Linux and Mac OS X.

Given a folder path, the web script lists the contents of that folder in the Alfresco content repository in abbreviated or verbose form depending on a user provided flag.

-   **[Creating a description document](../tasks/ws-desc-doc-create.md)**  
This task creates a web script description document for a Folder Listing web script.
-   **[Creating a controller script](../tasks/ws-controller-create.md)**  
The description document describes the Folder Listing web script and a JavaScript controller script implements its behavior. The controller establishes the folder to list from the invoked URI and query the Alfresco content repository for that folder ensuring error conditions are catered for.
-   **[Parsing the web script URI](../tasks/ws-uri-parse.md)**  
A web script is invoked when a URI is requested that matches one of the URI templates defined by the web script. The web script might need to access the requested URI to allow it to extract arguments that might have been passed in as URI query parameters or embedded as values in the URI path.
-   **[Calling Alfresco services](../concepts/ws-call-services.md)**  
Controller scripts have access to services provided by the Alfresco content application server. This allows a web script to query or perform operations against content residing in the Alfresco content repository. Services are exposed as root objects and each service provides its own API to program against.
-   **[Constructing the model](../concepts/ws-model-construct.md)**  
The controller script creates a model for subsequent rendering by a response template. A model is a map of values indexed by their name, which can be read from and written to by the controller script.
-   **[Creating a response template](../tasks/ws-respTemp-create.md)**  
The final stage of web script execution is to render a response back to the initiating client in an appropriate format based on the client's preference. A response template written in FreeMarker is responsible for rendering each format provided by the web script.
-   **[Registering and testing web scripts](../tasks/ws-register.md)**  
The web script index provides some administration of web scripts, in particular, for those developers creating new web scripts.
-   **[Creating multiple response templates](../tasks/ws-json-add.md)**  
A web script can support multiple response formats to allow it to be used by a variety of clients. For example, it can render an HTML response for human consumption in a web browser, and a JSON response for machine consumption by other clients.
-   **[Debugging a controller script](../tasks/ws-controller-debug.md)**  
The Alfresco content application server provides a built-in JavaScript Debugger that can be applied to web scripts. It is a useful tool for diagnosing the cause of issues and for stepping through the controller scripts of others to learn how they have implemented capabilities and used Alfresco services.

**Parent topic:**[Tutorials](../tasks/ws-tutorials.md)

