When you are getting the "websocket error", this page helps you find the reason.

> Note, all versions of BIMserver > 1.4 require tomcat version 8 (if you are using tomcat) because of a new implementation for websockets.

The webbased user interface that is currently being shipped with BIMserver is BIMvie.ws. BIMvie.ws requires a working WebSocket implementation in your browser (actually the BimServerJavaScriptApi has this dependency, which is itself a dependency of BIMvie.ws). 

Usually when web sockets do not work, it's either one of the following: 
- You are on a corporate network and the system administrators do not allow you to use Web Sockets (either intentionally or not) 
- You are using a proxy server that does not support websockets (or websockets have not been allowed/configured)
- You are using a very old browser (usually Internet Explorer). Internet Explorer is not supported, but newer versions tend to work anyways.
- Your application container (Tomcat, Jetty or etc...) does not support web sockets, this should be in the log files.

The web socket technology has been standardized in 2011.