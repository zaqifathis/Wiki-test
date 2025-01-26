# Oid

All objects in the BIMserver have a unique identifier, this is called the oid (for object identifier). This oid is of type long. Instances from the Ifc2x3 model (like IfcWall) have oids, but also instances of the Store model (like Project, Revision, User) have an oid. With unique we mean unique within the instance of the BIMserver

# Rid

Rid stands for Revision Identifier. Every object in the BIMserver that has an oid also has a rid. Rids are of type int. For instances of the Store model the rid is also -1 because they are currently not versioned. For other objects (in the Ifc2x3 model) the rid is incremented with each new revision of the object.

# The ConcreteRevision object

For every checkin, exactly 1 ConcreteRevision instance is created, this instance represents the actual checkin. But as you might have seen when you checkin a model on a subproject, revisions are also created on parent projects, that's where the Revision object comes into play. The ConcreteRevision object is the object that has the id attribute (not to be confused with oid), the id attribute is the same as the rid value for all objects comitted in that specific revision.

# The Revision object

For every checkin, at least one Revision object is created. Additional Revision object are created for all parent projects of the project the checkin was done on. Most of the time you will only have to use the Revision object.