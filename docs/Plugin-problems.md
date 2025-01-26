This page aims to explain the problems that we had with the plugin system so far, and how we hope to solve this in the (developer) releases after 1.5.108.

From version 1.5.108 on, strict plugin checking is enabled by default. What this means is that plugins have to be built for a specific version of BIMserver. Only plugins built for the exact BIMserver version you are using will be allowed to be installed.

> A quick side note: When installing plugins via the API (or BIMvie.ws), this strict checking applies since version 1.5.108. For the initial setup of a BIMserver this check was added in version 1.5.111.

This fixes 2 known problems:
- People that downloaded/installed a version of BIMserver a while ago would be tempted to upgrade installed plugins as new versions of plugins became available. Those plugins however were in most cases not compatible with the older version of BIMserver. There was no way to know this up-front. This also went wrong when settings up a new (but older) BIMserver, the setup process would simply download the latest version of each plugin.
- People that downloaded the newest version of BIMserver would see all kinds of plugins to be available, some of them more than a year old (some more than 2 years old!). These plugins usually don't work anymore since nobody is maintaining them.

This also introduces a few new problems, but hopefully less annoying ones:
- A smaller amount of plugins is usually available shortly after a new BIMserver release, since new builds have to be made for all plugins, linked specifically to this version of BIMserver

# So how is this better than before the plugin system was added?

Before the plugin system was implemented in BIMserver, all (what are now plugins) functionality was embedded in BIMserver itself. An important reason for introducing a plugin system was that we wanted a smaller core system, where we could do bugfixes etcetera of the plugins without releasing a new version of BIMserver.

This is still possible. A developer of a plugin can release a new version with bugfixes. When a plugin developer is sure that multiple versions of BIMserver are compatible with a new version, a Maven-specific dependency can be added. For example works with all versions between 1.5.106(inc) and 1.5.108(inc):

```xml
<dependency>
	<groupId>org.opensourcebim</groupId>
	<artifactId>pluginbase</artifactId>
	<version>[1.5.106, 1.5.108]</version>
</dependency>
```