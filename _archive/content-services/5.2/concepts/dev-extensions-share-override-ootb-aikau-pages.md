---
author: Alfresco Documentation
---

# Modifying Out-of-the-box Aikau Pages

The Share web application has a number of Aikau pages. These can be modified.

|Extension Point|Modifying Out-of-the-box Aikau Pages|
|---------------|------------------------------------|
|Support Status|[Full Support](https://docs.alfresco.com/support/concepts/su-product-lifecycle.html) via [Surf Extension Modules](dev-extensions-share-surf-extension-modules.md)|
|Architecture Information|[Share Architecture](dev-extensions-share-architecture-extension-points.md)|
|Description|The preferred way of modifying Aikau pages is by using [Surf Extension Modules](dev-extensions-share-surf-extension-modules.md) to target the Aikau widget that should be replaced or hidden. It is also possible to add widgets to a page this way. The Extension Modules section has all the details. Now, if we want to modify an existing page we need to grab hold of it in the Web Script controller, this will look like this:

```
var widget = widgetUtils.findObject(model.jsonModel.widgets, "id", "FCTSRCH_SEARCH_RESULTS_LIST");
if (widget && widget.config && widget.config.widgets) {
   widget.config.widgets.push( {
...   
```

This is all that is required to extend an existing JSON model. We're using `widgetUtils` to find the `FCTSRCH_SEARCH_RESULTS_LIST` widget. Once we have it, we simply push widgets into it.

|
|Deployment - App Server|-   tomcat/shared/classes/alfresco/web-extension/site-data/extensions/ \(Untouched by re-depolyments and upgrades\)

|
|[Deployment All-in-One SDK project](sdk-getting-started.md)|-   aio/share-jar/src/main/resources/alfresco/web-extension/site-data/extensions - Extension module goes here

|
|More Information|-   [Aikau Pages](dev-extensions-share-aikau-pages.md)
-   [Introduction to Aikau](aikau-intro.md)
-   [Aikau Widget Reference](https://dev.alfresco.com/resource/docs/aikau-jsdoc/) - this is the place to look for widgets that you can use in your dashlets.

|
|Tutorials|-   [Aikau Tutorials on GitHub](https://github.com/Alfresco/Aikau/blob/master/tutorial/chapters)
-   [Adding new AMD packages for Aikau Widgets](../tasks/dev-extensions-share-tutorials-amd-packages-via-extension.md)

|
|Developer Blogs|-   [Customizing Search Page](https://hub.alfresco.com/t5/alfresco-content-services-blog/adding-views-to-filtered-search/ba-p/292844)
-   [Aikau background and concepts](https://hub.alfresco.com/t5/alfresco-content-services-blog/latest-updates-to-share-and-surf/ba-p/289014)
-   [Deep dive into Dojo, Dijit, and Aikau development.](https://docs.google.com/document/d/1q25jA5EQ5PRYekr8tpM3ELlwOQ8Ht3Ng6D4VWsKoZtY/pub)

|

**Parent topic:**[Share Extension Points](../concepts/dev-extensions-share-extension-points-introduction.md)

