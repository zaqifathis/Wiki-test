SOAP is one of the three available channels to access the methods of the [Service Interfaces](Service-Interfaces).

This document describes how to use the SOAP interface of BIMserver 1.2

External applications can interact with the BIMserver via SOAP (and [Interfaces other protocols]). For SOAP you'll need a WSDL file which describes all the methods/objects that are available. The BIMserver provides several interfaces and two versions of SOAP (1.1 and 1.2).

_soapversion_ can be either "soap11" or "soap12".<br/>
_protocol_ can be either "http" or "https".<br/>
_address_ can be an IP address or (sub)domain name.<br/>
_port_ can be omitted (80) or an integer.<br/>

| URL |
| _protocol_://_address_:_port_/_soapversion_/AdminInterface |
| _protocol_://_address_:_port_/_soapversion_/AuthInterface |
| _protocol_://_address_:_port_/_soapversion_/MetaInterface |
| _protocol_://_address_:_port_/_soapversion_/PluginInterface |
| _protocol_://_address_:_port_/_soapversion_/ServiceInterface |
| _protocol_://_address_:_port_/_soapversion_/SettingsInterface |
| _protocol_://_address_:_port_/_soapversion_/Bimsie1AuthInterface |
| _protocol_://_address_:_port_/_soapversion_/Bimsie1LowLevelInterface |
| _protocol_://_address_:_port_/_soapversion_/Bimsie1NotificationInterface |
| _protocol_://_address_:_port_/_soapversion_/Bimsie1NotificationRegistryInterface |
| _protocol_://_address_:_port_/_soapversion_/Bimsie1RemoteServiceInterface |
| _protocol_://_address_:_port_/_soapversion_/Bimsie1ServiceInterface |

Example WSDL URL: http://localhost:8080/soap12/ServiceInterface?wsdl

# SOAP Stack

To use SOAP you will need to have a "SOAP Stack" for your programming language. A SOAP Stack is just a library containing all the logic for the SOAP protocol. For most programming languages there are multiple, so it's up to you to choose one.

Your SOAP Stack is probably able to generate a bunch of code for your programming language based on a WSDL file/url, this is not required but makes your life a lot easier.

# Authentication

The newest version of the BIMserver SOAP interface can handle two kinds of token passing.

## HTTP Session Based

Make sure to enable session management in your client, otherwise the server won't be able to remember who you are and you will get this message:
{{{
"Authentication required for this call"
}}}

For Apache CXF, you can enable sessions this way:
{{{
((javax.xml.ws.BindingProvider)port).getRequestContext().put(BindingProvider.SESSION_MAINTAIN_PROPERTY, true);
}}}

For .Net based clients, this can be done this way:
{{{
client = new BiMserver.ServiceInterfaceService();
client.CookieContainer = new System.Net.CookieContainer();
}}}

## SOAP Header Based

Some clients won't be able to alter the HTTP Headers, so an alternative is to use the SOAP Header. The BIMserver supports both ways without any server side configuration.

After you call login/autlogin, you will receive a token, this token must be included in all subsequent calls in the header. The SOAP message will have to look like this:

```
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
	<soap:Header>
		<ns2:token xmlns:ns2="uri:org.bimserver.shared">
<token>70230BC83C052A3D1312FC516F9E3F5E</token>
		</ns2:token>
	</soap:Header>
	<soap:Body>
		<ns2:getAllUsers xmlns:ns2="http://shared.bimserver.org/"
			xmlns:ns3="uri:org.bimserver" />
	</soap:Body>
</soap:Envelope>
```

To enable this kind of functionality in Apache CXF (Java)
```java
	Token token = serviceInterface.getCurrentToken();
	List<Header> headers = new ArrayList<Header>();
	Header sessionHeader = new Header(new QName("uri:org.bimserver.shared", "token"), token, new JAXBDataBinding(Token.class));
	headers.add(sessionHeader);
	((BindingProvider) serviceInterface).getRequestContext().put(Header.HEADER_LIST, headers);
```

The [BimServerClientLib BimServerClientLib] has an option to switch between both ways. When connection with the method connectSoap, the second argument indicates weather to use the first (false) or second (true) method.

# GeneratedClient
From version 1.1, there is a "GeneratedSoapClient" project in SVN. This project contains generated source code based on Apache CXF and the latest BIMserver "ServiceInterface". It's just a convenience project.