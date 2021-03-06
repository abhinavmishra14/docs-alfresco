---
author: Alfresco Documentation
---

# Subsystems

Subsystems are configurable modules responsible for a piece of functionality in the Alfresco content management system. It is possible to implement an extension as a custom subsystem.

|Information|Subsystems|
|-----------|----------|
|Support Status|[Full Support](http://docs.alfresco.com/support/concepts/su-product-lifecycle.html)|
|Architecture Information|[Platform Architecture](../concepts/dev-platform-arch.md)|
|Description|Subsystems are configurable modules responsible for a piece of functionality in the Alfresco content management system. The functionality is often optional such as the IMAP server or can have several different implementations, such as authentication.

 A subsystem can be thought of as a mini-server that runs embedded within the main Alfresco server. A subsystem has the following characteristics:

 -   It can be started, stopped, and configured independently of the Alfresco server during runtime
-   It has its own isolated Spring application context and configuration

 Subsystems are independent processes that can be brought up and down. This design lets an administrator of the system change a single configuration without having to bring down the entire Alfresco system. The advantages are reliability and availability.

 Examples of Alfresco subsystems include:

 -   **Audit**: Configuration of audit parameters
-   **Authentication**: Contains different authentication subsystems such as LDAP
-   **E-mail**: SMTP support for sending e-mails
-   **File servers**: CIFS, FTP
-   **Google Docs**: Google Docs integration
-   **IMAP**: Internal IMAP server
-   **Open Office transformations**: Helps converting office documents to text
-   **Search**: Search system integration, Lucene, Solr, None
-   **Synchronization**: LDAP synchronization settings
-   **Sys admin**: It allows real-time control across some general repository parameters
-   **Third-party**: Owns the SWFTools and ImageMagick content transformers

 Implementing an extension as a subsystem allows a more fully decoupled customization. It is, for example, possible to disable the customization at run time.

 The first thing we need to do before starting the implementation of our custom subsystem is to decide what category and type we want to use:

 -   **Category** - this is a broad description of the subsystem, such as Search or Authentication
-   **Type** - this is a particular flavour \(i.e. implementation\) of the subsystem category, such as Solr and Lucene or LDAP and Kerberos

 Implementing a type of a subsystem category is pretty much the same thing as implementing a [platform customization](../concepts/dev-platform-extension-points.md), the customization is just deployed a bit differently then we are used to. Note also that subsystems are platform specific and a Share customization cannot be implemented as a subsystem.

 For demonstration purposes we will assume that we have started to implement integrations with different Semantic content products, and the first one is a Cloud based Named Entities service. This service can be used to send in a text and get back named entities such as people, companies, and countries.

 The category for this custom subsystem will be called Semantic and the type for the first implementation will be called Named Entities. A subsystem type implementation is delivered as a [Repo AMP](../tasks/alfresco-sdk-tutorials-amp-archetype.md).

 The first thing we should always do when implementing a subsystem is to **implement the platform extension** \(that should be deployed as a subsystem\) in the normal way and deploy as a Repo AMP or Simple JAR extension. When the extension works fine deployed as a Repository AMP we can continue and turn it into a subsystem deployment.

 In our case we got the following simple class representing our semantic named entities service:

 ```
public class DummyNamedEntitiesServiceImpl implements NamedEntitiesService {
    private static final Log LOG = LogFactory.getLog(DummyNamedEntitiesServiceImpl.class);

    /**
     * Properties used to connect to the remote named entities semantic cloud service.
     * Not used in this dummy implementation.
     */
    private String remoteServiceHostname;
    private String remoteServiceAipKey;
    private String remoteServicePort;

    /**
     * Service registry for access to Alfresco public API
     */
    private ServiceRegistry serviceRegistry;

    public void setRemoteServiceHostname(String remoteServiceHostname) {
        this.remoteServiceHostname = remoteServiceHostname;
    }

    public void setRemoteServiceAipKey(String remoteServiceAipKey) {
        this.remoteServiceAipKey = remoteServiceAipKey;
    }

    public void setRemoteServicePort(String remoteServicePort) {
        this.remoteServicePort = remoteServicePort;
    }

    public void setServiceRegistry(ServiceRegistry serviceRegistry) {
        this.serviceRegistry = serviceRegistry;
    }

    public void extractNamedEntities(NodeRef nodeRef) {
        if (serviceRegistry.getNodeService().exists(nodeRef)) {
            LOG.info("Extracting named entities for node [nodeRef=" + nodeRef + "]");

            // Dummy implementation that does nothing
            // In a real implementation we would:
            // 1) extract the text for the node via for example NodeService and ContentService
            // 2) call the remote service with the text to get the named entities
            // 3) add a custom NamedEntities aspect to the node with the extracted entities, such as person, company, country etc.
        } else {
            LOG.error("Node does not exist [nodeRef=" + nodeRef + "]");
        }
    }

}
```

 So this class doesn't actually do anything, it is just a dummy implementation as this extension point is about subsystem implementations and deployments, not about doing an integration with a semantic service in the Cloud. The class is registered in the usual place, such as /repo-amp/src/main/amp/config/alfresco/module/repo-amp/context/service-context.xml, which is brought in by the repo-amp/src/main/amp/config/alfresco/module/repo-amp/module-context.xml context file, which in turn is picked up by the Alfresco Spring context. The bean definition looks like this:

 ```
<bean id="namedEntitiesService"
       class="org.alfresco.tutorial.semantic.namedentities.DummyNamedEntitiesServiceImpl">
     <property name="remoteServiceHostname"><value>${namedentities.remote.service.hostname}</value></property>
     <property name="remoteServicePort"><value>${namedentities.remote.service.port}</value></property>
     <property name="remoteServiceAipKey"><value>${namedentities.remote.service.api.key}</value></property>
     <property name="serviceRegistry">
         <ref bean="ServiceRegistry" />
     </property>
 </bean>
```

 We inject the properties that are needed to connect to the remote Cloud service and we inject the Alfresco Service Registry so we can get to the Alfresco public API. This service would now be available to any other bean in the Alfresco Spring context. However, when we turn this extension into being deployed as a subsystem the `namedEntitiesService` bean will no longer be available outside of the subsystem Spring context. This is important, and we need to provide another way of accessing the service that the subsystem provides, such as via a Web Script.

 The Repo Web Script descriptor looks like this:

 ```
<webscript>
    <shortname>Extract Named Entities</shortname>
    <description>This Web Script is used to call the Named Entities Service to extract named entities from a text</description>
    <url>/tutorial/extractentities?nodeId={nodeId}</url>
    <authentication>user</authentication>
</webscript>
```

 And it is implemented with a Java controller as follows:

 ```
public class NamedEntitiesWebScript extends DeclarativeWebScript {
    private static final String NODEID_PARAM_NAME = "nodeId";

    /**
     * Service registry for access to Alfresco public API
     */
    private ServiceRegistry serviceRegistry;

    /**
     * Named entities service to call to extract entities for text
     */
    private NamedEntitiesService namedEntitiesService;

    public void setServiceRegistry(ServiceRegistry serviceRegistry) {
        this.serviceRegistry = serviceRegistry;
    }

    public void setNamedEntitiesService(NamedEntitiesService namedEntitiesService) {
        this.namedEntitiesService = namedEntitiesService;
    }

    @Override
    protected Map<String, Object> executeImpl(WebScriptRequest webScriptRequest, Status status) {
        Map<String, Object> model = new HashMap<String, Object>();

        String nodeId = webScriptRequest.getParameter(NODEID_PARAM_NAME);

        if (StringUtils.isBlank(nodeId)) {
            status.setCode(400, "Failed entity extraction: required node ID data has not been provided");
            status.setRedirect(true);
        } else {
            NodeRef nodeRef = new NodeRef(StoreRef.STORE_REF_WORKSPACE_SPACESSTORE + "/" + nodeId);
            if (!serviceRegistry.getNodeService().exists(nodeRef)) {
                status.setCode(404, "Failed entity extraction: no node found for node reference:" + nodeRef);
                status.setRedirect(true);
            } else {
                namedEntitiesService.extractNamedEntities(nodeRef);

                model.put("message", "Successfully completed named entities extraction");
                model.put("nodeId", nodeId);
            }
        }

        return model;
    }
}
```

 The Web Script controller is registered with the following Spring bean definition:

 ```
<bean id="webscript.namedentities.get"
       class="org.alfresco.tutorial.semantic.namedentities.NamedEntitiesWebScript"
       parent="webscript">
    <property name="namedEntitiesService">
        <ref bean="namedEntitiesService" />
    </property>
    <property name="serviceRegistry">
        <ref bean="ServiceRegistry" />
    </property>
</bean>
```

 We inject the `namedEntitiesService` into the Web Script controller so it can be called. The Web Script is invoked via a simple HTTP URL \(e.g. http://localhost:8080/alfresco/service/tutorial/extractentities?nodeId=34e4a654-b45e-4be1-80db-25da1e4c82a7\), which is the way that we can access the services of this subsystem without direct access to the Spring context. The subsystem implementation will be decoupled from the rest of the Alfresco Spring context.

 Now that we have a standard working platform \(i.e. Repository\) extension we can turn it into a subsystem implementation and deployment.

 To start our subsystem implementation create a new category called **Semantic**. We do this by creating a directory under the /alfresco/subsystems directory. Then we create the type directory **namedEntities** as a subdirectory to Semantic so we end up with the /alfresco/subsystems/Semantic/namedEntities directory path. In the namedEntities directory we create the following two files:

 ```
namedentities-semantic.properties
namedentities-semantic-context.xml
```

 The properties file will contain the configuration that our extension needs and the context file will register all the Spring beans that are needed for the complete subsystem implementation. For these files to be picked up they need to end in .properties and -context.xml. So we could have used completely different file-names here if we wanted to, as long as they have the correct suffix. In the properties file we add the following properties:

 ```
namedentities.remote.service.hostname=host1.acme.com
namedentities.remote.service.port=80
namedentities.remote.service.api.key=HHS44HGS22672BDSSSS444SSSS
```

 In this case these properties point to a fictional Cloud service that provide named entities extraction. We should include all properties needed by the platform extension. In this case the properties are there for demonstration purposes, they will actually not be used to connect to a remote service, just demonstrate how to inject properties into the extension code.

 After this it is time to **move our Spring beans** from the service-context.xml file into the namedentities-semantic-context.xml Spring context file. We are basically moving our beans out of the general Alfresco Spring context into a specific subsystem Spring context, where they can access the Alfresco Spring context \(such as `serviceRegistry`\) but not the other way around.

 Now, to kick off the Spring context for the subsystem category and type add the following Spring bean to the service-context.xml Spring context file \(note that we add this bean to the Spring context known to the Alfresco server, so it can start the subsystem\):

 ```
<bean id="namedEntitiesService.subsystem.type"
       class="org.alfresco.repo.management.subsystems.ChildApplicationContextFactory"
       parent="abstractPropertyBackedBean">
     <property name="category">
         <value>Semantic</value>
     </property>
     <property name="typeName">
         <value>namedEntities</value>
     </property>
     <property name="instancePath">
         <list>
             <value>namedEntities</value>
         </list>
     </property>
     <property name="autoStart">
         <value>true</value>
     </property>
 </bean>
```

 There are three different kinds of subsystem Spring context factories, including the one used above:

 -   `ChildApplicationContextFactory` - single extension instance deployment, can be stopped, reconfigured and started at runtime via JMX
-   `SwitchableApplicationContextFactory` - allows swapping of subsystem type implementations \(i.e. Lucene vs Solr\)
-   `DefaultChildApplicationContextManager` - Chain or pick subsystem type \(i.e. Authentication chain\)

 If we start up an Alfresco Enterprise edition with this subsystem implementation we should see the following in the log when it is mounted:

 ```
2016-02-15 15:45:56,901 INFO [management.subsystems.ChildApplicationContextFactory] [localhost-startStop-1] Starting 'Semantic' subsystem, ID: [Semantic, namedEntities] 
2016-02-15 15:45:56,925 INFO [management.subsystems.ChildApplicationContextFactory] [localhost-startStop-1] Startup of 'Semantic' subsystem, ID: [Semantic, namedEntities] complete 
```

 We can manage it from JConsole:

 ![](../images/dev-extensions-repo-custom-subsystem-jconsole.png)

 From JConsole we can re-configure, start, and stop our new custom subsystem.

 However, if we actually call the Web Script it will not work. This is because the Web Script descriptor is registered with the Alfresco Web Script container and when it is looking for any controller belonging to the Web Script it does not see the subsystem context with the controller bean definition. So when the Web Script is called the Alfresco log will show an error where `nodeId` is unknown, basically the Web Script container has gone directly to the Web Script template and bypassed the controller.

 The way we can fix this is to move the Web Script controller Spring bean definition back into the service-context.xml file so it is known to Alfresco and the Web Script container. But then we got the problem that the controller Spring bean will not load as it knows nothing about the subsystem `namedEntitiesService` bean. So, is there a way to expose a Spring bean in a subsystem context so it can be used in the main Alfresco parent Spring context? Yes there is, we can use a proxy approach, define the following bean in the service-context.xml file:

 ```
<bean id="namedEntitiesService.proxy" class="org.alfresco.repo.management.subsystems.SubsystemProxyFactory">
     <property name="sourceApplicationContextFactory">
         <ref bean="namedEntitiesService.subsystem.type" />
     </property>
     <property name="sourceBeanName">
         <value>namedEntitiesService</value>
     </property>
     <property name="interfaces">
         <list>
             <value>org.alfresco.tutorial.semantic.namedentities.NamedEntitiesService</value>
         </list>
     </property>
 </bean>
```

 This `namedEntitiesService.proxy` bean definition creates a proxy for the `namedEntitiesService` bean in the subsystem context. We can then change the Web Script controller bean definition to use it \(also in service-context.xml\):

 ```
<bean id="webscript.namedentities.get"
       class="org.alfresco.tutorial.semantic.namedentities.NamedEntitiesWebScript"
       parent="webscript">
     <property name="namedEntitiesService">
         <ref bean="namedEntitiesService.proxy" />
     </property>
     <property name="serviceRegistry">
         <ref bean="ServiceRegistry" />
     </property>
 </bean>
```

 The subsystem Spring context file namedentities-semantic-context.xml now only contains the following bean definition:

 ```
<bean id="namedEntitiesService"
       class="org.alfresco.tutorial.semantic.namedentities.DummyNamedEntitiesServiceImpl">
     <property name="remoteServiceHostname"><value>${namedentities.remote.service.hostname}</value></property>
     <property name="remoteServicePort"><value>${namedentities.remote.service.port}</value></property>
     <property name="remoteServiceAipKey"><value>${namedentities.remote.service.api.key}</value></property>
     <property name="serviceRegistry">
         <ref bean="ServiceRegistry" />
     </property>
 </bean>
```

 Using this proxy approach also has the benefit that if the subsystem is stopped it will be automatically started if we call any method in the `NamedEntitiesService` interface.

|
|Deployment - App Server|-   tomcat/shared/classes/alfresco/extension/subsystems/\{category\}/\{type\} - extension properties and Spring Beans
-   The Java class implementation does not lend itself very well to be directly deployed directly to the application server. Use a Repo AMP SDK project instead, see below.

|
|[Deployment - SDK Project](../tasks/alfresco-sdk-tutorials-amp-archetype.md)|-   repo-amp/src/main/amp/config/alfresco/subsystems/\{category\}/\{type\} - extension component properties and Spring Beans
-   repo-amp/src/main/java - extension implementation classes

|
|More Information|-   [Configuring Alfresco Subsystems](../concepts/subsystem-intro.md)

|
|Sample Code|-   [A sample implementation of a custom subsystem](https://github.com/Alfresco/alfresco-sdk-samples/tree/alfresco-51/all-in-one/custom-subsystem-repo)

|
|Tutorials|None|
|Alfresco Developer Blogs|None|

**Parent topic:**[Platform extension points](../concepts/dev-platform-extension-points.md)

