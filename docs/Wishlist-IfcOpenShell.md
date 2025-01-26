# IFC4 support

# Generate/acquire geometry for specific representation

At the moment IfcOpenShell generates geometry per IfcProduct (from a BIMserver perspective). When for example an IfcWall is made from different materials (but not semantically modelled that way using assemblies), the different layers can have their own colors, but they won't be recognizable as different layers in the viewer.

## Performance

In order to have:
- Low/manageable memory usage
- Simultaneous Multi-core computation

BIMserver now splits the original IFC file in smaller IFC files, data that is not relevant for geometry generation is omitted (for example property sets). This is now done on an IfcProduct level (usually 10 at-a-time). For objects that reuse geometry and that have

# Speed