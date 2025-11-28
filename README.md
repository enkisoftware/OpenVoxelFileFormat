# OpenVoxelFileFormat
Open Voxel File Format Specification

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
   2. Bone animations (per voxel weights can be included in voxel data)
   3. Frame-animations (stretch goal)
   4. Multiple animation tracks
1. Materials
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

See the [Voxel data format issue](https://github.com/enkisoftware/OpenVoxelFileFormat/issues/4)

## Implementation Notes

The glTF file format with an extension for voxel data and material palettes meets these goals. [The .glb binary format](https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html#binary-gltf-layout) is limited to a `uint32` length and single `uint32` length binary buffer, which is likely too short for large voxel scenes. Thus either external data (which is part of the spec) may need to be used or an alternative binary format might be required. A compressed zip like format may be a good way to handle bundling a gltf file and its associated binary data files.

[A voxel data extension has been propossed for glTF, PR #2496](https://github.com/KhronosGroup/glTF/pull/2496) however this does not look like it would meet our Voxel Data goals, but it might be worth experimenting with to check.


See the issue [Implementation decision: modify existing format or custom](https://github.com/enkisoftware/OpenVoxelFileFormat/issues/3)
