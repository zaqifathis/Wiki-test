"Team projects sets" is a feature  of Eclipse that allows you to quickly setup a large list of projects from multiple GIT repositories.

# Recipe

* Start with an empty workspace
* Go to File | Import | Team | Team Project Set
* Select the [TPS](https://raw.githubusercontent.com/opensourceBIM/BIMserver/master/Documentation/files/projectSet.psf) and import it, this can take a while

> Sometimes eclipse has a problem loading big projects. If you have a lot of compile/dependency problems right away, you can try the following two things:

* Project | Clean... (and then clear all projects), this will issue a complete recompile of everything
* Right Mouse on a project | Maven | Update Project ... | Select all projects => Ok

# Currently included repositories/projects

* All core BIMserver projects (BimServer, BimServerJar, BimServerWar etc...)
* BIMserver IfcOpenShell plugin
* IfcPlugins
* BinarySerializers
* BIMvie.ws
* BIMsurfer (v1)
* BIMserver JavaScript API

# Content of the Team Project Set
```xml
<?xml version="1.0" encoding="UTF-8"?>
<psf version="2.0">
<provider id="org.eclipse.egit.core.GitProvider">
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,BimServer"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,BimServerClientLib"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,BimServerJar"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,BimServerWar"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMsurfer.git,V1,."/>
<project reference="1.0,https://github.com/opensourceBIM/bimvie.ws.git,master,."/>
<project reference="1.0,https://github.com/opensourceBIM/BinarySerializers.git,master,BinarySerializers"/>
<project reference="1.0,https://github.com/opensourceBIM/Console.git,master,Console"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,Documentation"/>
<project reference="1.0,https://github.com/opensourceBIM/IfcOpenShell-BIMserver-plugin.git,master,."/>
<project reference="1.0,https://github.com/opensourceBIM/IfcPlugins.git,master,IfcPlugins"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver-JavaScript-API.git,master,."/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,PluginBase"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,Shared"/>
<project reference="1.0,https://github.com/opensourceBIM/BIMserver.git,master,Tests"/>
</provider>
</psf>
```