Update: This is not the current roadmap

This is just a proposal, lots of changes expected

September 2015
- Create roadmap
- Standardize on topicId, downloadId, laid, longActionId etc... and document
- Create calls to get information about "background" processes, also implement in BIMvie.ws
- Extend Metrics with percentile info and more
- Write more documentation (maybe work on scenarios)
- Write a test that creates a 100GB database

Oktober 2015
- Convert to maven, publish on maven repository
- Load plugins on first use, to improve startup-time and memory consumption
- Maybe don't unpack JAR's on disk for plugins
- Setup a plugin repository (possibly using maven)
- Move non-essential plugins to different git repositories
- Deprecate code that will be removed
- Remove Bimsie1 namespace from all code (also update bimserverapi.js, BIMvie.ws, BIMsurfer etc...)

November 2015
- Remove query-plugin extensionpoint
- Implement indices for model-data
- Move GUID/Name indices to generic indexing system
- Implement useful query method, replace current query methods with [New-query-language](New-query-langage)

December 2015
- Remove [deprecated code](Deprecated)
- Add get/list/read methods to low level interface, remove data object calls
- Notifications on object-level
- Implement useful functional logging system (inc. notifications, log on disk)

January 2016
- Move usage of EMF model from server side to client side

Februari 2016
- Proof of concept scalable database implementation (branch)

Items to put on roadmap:
- Create HA ability (at least with 2 servers, 1 in standby)