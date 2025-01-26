Since version 1.5.88 any BIMserver instance is capable of running services on models that are not necessarily stored on that specific instance. This is utilizing the BimBot interface which is a lot leaner than the previous protocol for inter-BIMserver communication.

The previous protocol basically only support sending/receiving notifications which contained the proper credentials to subsequently use the BIMserver API to query the model. Services could optionally attach extended data to a revision, but were not required to do so.

The BimBots interface is a little more strict in the sense that it has a pre-defined input _and_ output. For this reason, writing services in BIMserver that can be run as a BimBot is slightly different. Nontheless existing services can be easily retrofitted and that's what this page is about.

# BimBots interface

> Note, when talking about an interface on this page we mean an actual Java interface. The BimBots protocol itself however is language independent (see: [external services](External-services.md)).

> Note this interface is still subject to change.

The main interface you have to implement is 'BimBotsServiceInterface'. There are currently 3 methods you should provide.

## getAvailableInputs

```java
Set<SchemaName> getAvailableInputs();
```

This should return a list of SchemaName (enum) values that this service can interpret as an input. In most cases this will be either IFC_STEP_2X3TC1, IFC_STEP_4, IFC_XML_2X3TC1 or IFC_XML_4.

## getAvailableOutputs

```java
Set<SchemaName> getAvailableOutputs();
```

## runBimbot

```java
BimBotsOutput runBimBot(BimBotsInput input, SObjectType settings) throws BimBotsException;
```

This method is where the service is called. The input object will usually contain an already parsed ifc model (which you can get by calling `getIfcModel`).

The result of this function should be of type BimBotsOutput, which should contain the actual data, the schema used and other info such as the ContentType.

# Converting an existing service

Services that have been written earlier (for example subclasses of AbstractAddExtendedDataService or AbstractModifyRevisionService) should now subclass BimBotAbstractService.

New services can also just implement BimBotsServiceInterface, but then the services won't be available as a service in the old style.

Example: https://github.com/opensourceBIM/DemoPlugins/blob/master/DemoPlugins/src/org/bimserver/demoplugins/bimbotdemo/BimBotDemoService.java
