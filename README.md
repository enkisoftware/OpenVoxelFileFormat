# OpenVoxelFileFormat
Open Voxel File Format Discussion and Specification

This is intended as a simple format for exchanging voxel scenes, models and animations etc. There will be a binary voxel data representation with some compression, but extremely highly compressed voxel data is not a primary goal.

## Status
The format is currently in the requirements definition and early design stage.

## Requirements

Note that these are currently fairly high level to permit some flexibility in the implementation.

### Scene Description

1. As simple as possible - implementations should be able to be lightweight
1. Open and extensible
1. Arbitrary transforms
1. Animation:
   1. Transform animations
   2. Bone animations
   3. Frame-animations (stretch goal)
   4. Multiple animation tracks
1. PBR material properties including those needed for transmissive volumes
   1. Multiple material palettes
1. Voxel Data (see below)

### Voxel Data

1. Voxels with arbitrary data/attributes including indexed materials (similar to how vertex data can be specified, see below)
1. No arbitrary limitation on size of voxel volume
1. Ability to efficiently handle sparsity
1. Relatively efficient but simple storage and serialization performance

Simplified example Voxel attributes, can potentially be interleaved or separate streams:
```
Usage,          Number, Type
RGBA_COLOR,     4,      UNSIGNED_BYTE
MATERIAL_INDEX, 1,      UNSIGNED_BYTE
MATERIAL_INDEX, 1,      UNSIGNED_SHORT
DENSITY,        1,      FLOAT32
WEIGHTS_n       4,      UNSIGNED_NORMALIZED_BYTE
```

## Implementation Notes

The glTF file format with an extension for voxel data and material palettes meets these goals. Due to it's limitations for the size of binary data in the `glb` format external voxel assets may be required, and thus in practice we may want to use a compressed archive container or extend the glb binary blob spec.

[A voxel data extension has been propossed for glTF, PR #2496](https://github.com/KhronosGroup/glTF/pull/2496) however this does not look like it would meet our Voxel Data goals, but it would be might worth experimenting with to check.
