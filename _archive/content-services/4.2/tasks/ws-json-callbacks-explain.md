---
author: Alfresco Documentation
---

# Understanding how the JSON callback works

The easiest way to understand the callback example is to invoke the Hello User web script directly and interrogate the response.

-   Type the following in the command line: `curl -uadmin:admin "http://localhost:8080/alfresco/s/hellouser.json?alf_callback=showGreeting"`

    This mimics the web script invocation made in the callback.html file.

    The response is:

    showGreeting\(\{greeting: "hello", user: "admin"\}\)

    This is simply the vanilla Hello User web script response passed as an argument to the function named `showGreeting` as defined by the `alf_callback` query parameter. The full response is treated as JavaScript by the web browser, which executes it.


**Parent topic:**[Creating a Hello User web script with authentication](../tasks/ws-hello-user-create.md)

