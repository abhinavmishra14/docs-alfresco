---
author: Alfresco Documentation
---

# Modifying Out-of-the-box Surf Widgets

The Share web application pages and dashlets are built up of widgets. Sometimes it is necessary to modify these.

|Extension Point|Modifying Out-of-the-box Surf Widgets|
|---------------|-------------------------------------|
|Architecture Information|[Share Architecture](dev-extensions-share-architecture-extension-points.md).|
|Description|The preferred way of modifying Surf widgets is by using [Surf Extension Modules](dev-extensions-share-surf-extension-modules.md) to target the component that has the widget that should be replaced or hidden. It is also possible to add widgets to a page this way. The Extension Modules section has all the details.

|
|Deployment - App Server|-   tomcat/shared/classes/alfresco/web-extension/site-data/extensions/ \(Untouched by re-depolyments and upgrades\)

|
|[Deployment - SDK Project](../tasks/alfresco-sdk-tutorials-share-amp-archetype.md)|-   share-amp/src/main/amp/config/alfresco/web-extension/site-data/extensions/ - Store extension modules here

|
|More Information|-   [Surf Widgets](dev-extensions-share-surf-widgets.md)

|
|Tutorials|-   See [Surf Extension Modules](dev-extensions-share-surf-extension-modules.md)

|
|Alfresco Developer Blogs||

**Parent topic:**[Share Extension Points](../concepts/dev-extensions-share-extension-points-introduction.md)

