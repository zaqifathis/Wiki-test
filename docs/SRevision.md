This page describes a Revision from an API perspective. When using the API, all BIMserver objects are prefixed with an "S". So this object's name is "SRevision".

Some of the most used fields of this object are:
- oid, This is a unique identifier for this object. This is what is also called an "roid" when referenced outside of the Revision context. Make sure to not mix this id up with the next field
- id, this is an incrementing integer, it starts at 1 for the first revision of a project, 2 for the second etc...

The SRevision object is referred to from serveral other objects. It's referred from [SProject](SProject) 2 times:
- [SProject](SProject).lastRevisionId, it is referring to SRevision.oid
- [SProject](SProject).revisions, this is a list of Long, it is referring to SRevision.oid