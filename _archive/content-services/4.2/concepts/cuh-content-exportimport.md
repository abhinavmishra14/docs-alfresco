---
author: [Alfresco Documentation, Alfresco Documentation]
audience: 
category: 
option: 
---

# Exporting and importing content

Exporting and importing content allows you to copy a space and its contents from one location in Alfresco to another, or from one Alfresco repository to another.

This differs from a simple copy of a space from one location to another because in addition to copying sub spaces and content items, it includes all rules, workflow, properties, and metadata associated with that space.

Exporting a space and its contents bundles all the content into an Alfresco Content Package \(ACP\). Importing the ACP into a new location expands the package to the original structure of the space.

**Note:** There are a number of limitations when using export and import:

-   You can perform an export or import between compatible versions of Alfresco only. This usually means using the same version of Alfresco.
-   You can use export and import on folders and contents only. For example, you can't use this function to export or import a site. If you need to export a site, see [Export Web Site](../references/RESTful-SiteSite-exportGet.md) for more information.

-   **[Exporting a space and its contents](../tasks/tuh-content-export.md)**  
Export a space to copy the space, its contents, and all rules, workflow, properties, and metadata associated with the space to a new location within Alfresco.
-   **[Importing the ACP file into a space](../tasks/tuh-content-import.md)**  
Use the Import function to copy an exported space to a new location within Alfresco.

**Parent topic:**[Working with content](../concepts/cuh-content.md)

