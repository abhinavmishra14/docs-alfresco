---
author: Alfresco Documentation
---

# Surf framework

When building new presentation templates or web components, developers can choose to use the FreeMarker and JavaScript technologies. These are the default and preferred way to build high performance and lightweight web parts. They are easy to build and require no server restarts.

The availability of these APIs speeds the time it takes to develop new functionality. Most Surf platform features are available as root scope JavaScript and FreeMarker objects. Developers are able to work with the full range of objects available in the Surf framework. Objects represent entities such as component bindings, pages, templates, the request context, users, remote connections, and credential management.

**Important:** The FreeMarker Template API and the JavaScript API use a common object model. This means that the objects available to the JavaScript API are very similar \(in most cases, identical\) to those made available by the FreeMarker API. It is highly recommended that the standard development pattern of the logic work being performed in JavaScript and presentation work being performed in FreeMarker should be followed where possible.

The Surf platform FreeMarker Template Processor provides capabilities similar to those provided by the repository FreeMarker Engine. **It does not, however, provide direct access to the repository concepts, such as nodes, properties, or aspects that developers of repository tier web scripts will be familiar with.**

The Surf platform web script runtime extends the templating and scripting capabilities that are already provided by the web script runtime, providing additional web-tier related root-scoped API objects.

**Parent topic:**[Spring Surf API](../references/APISurfPlatform-intro.md)

