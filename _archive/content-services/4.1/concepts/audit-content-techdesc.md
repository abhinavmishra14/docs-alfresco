---
author: [Alfresco Documentation, Alfresco Documentation]
source: 
audience: 
category: Administration
option: audit
---

# Content auditing technical overview

The data producer `org.alfresco.repo.audit.access.AccessAuditor` gathers together lower events into user recognizable events. For example, the download or preview of content are recorded as a single read. Similarly the upload of a new version of a document is recorded as a single create version. By contrast the `AuditMethodInterceptor` data producer typically would record multiple events.

A default audit configuration file located at <alfresco.war\>/WEB-INF/classes/alfresco/audit/alfresco-audit-access.xml is provided that persists audit data for general use. This may be enhanced to extract additional data of interest to specific installations. For ease of use, login success, login failure and logout events are also persisted by the default configuration.

Default audit filter settings are also provided for the `AccessAuditor` data producer, so that internal events are not reported. These settings may be customized \(by setting global properties\) to include or exclude auditing of specific areas of the repository, users or some other value included in the audit data created by `AccessAuditor`.

No additional functionality is provided for the retrieval of persisted audit data, as all data is stored in the standard way, and so is accessible using the `AuditService` search, audit web scripts, database queries, and Alfresco Explorer `show_audit.ftl` preview.

-   **[Example audit trail](../tasks/audit-content-example.md)**  
The following is an example audit trail.
-   **[Using Alfresco Explorer to view the example audit trail](../tasks/audit-content-explorer.md)**  
The audit trail of individual content may be viewed from within Alfresco Explorer.
-   **[Enabling auditing of content](../tasks/audit-enable.md)**  
This section describes how to enable auditing of these events and sub events
-   **[Default audit filter settings](../concepts/audit-filter-settings.md)**  
The following properties are set by default to discard events where the user is *null* or *"System"*, the content or folder path is under *"/sys:archivedItem"* or under *"/ver:"* or the node type is not *"cm:folder"*, *"cm:content"* or *"st:site"*.
-   **[Audit data generated by AccessAuditor](../concepts/audit-accessauditer.md)**  
Folder and content changes generate the following audit data structure. Elements are omitted if not changed by the transaction.
-   **[Persisted audit data](../concepts/audit-persisted.md)**  
The default audit configuration file alfresco-audit-access.xml only copies the following audit data elements. It also adds `login`, `loginFailure` and `logout` persisted data.

**Parent topic:**[Content auditing](../concepts/audit-content.md)

**Related information**  


[Audit filters](audit-filters.md)

