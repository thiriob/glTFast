# Features

*glTFast* requires Unity 2019.4 or newer.

### Legend

- ✅ Fully supported
- ☑️ Partially supported
- ℹ️ Planned (click for issue)
- ⛔️ No plan to support (click for issue)
- `?`: Unknown / Untested
- `n/a`: Not available

## Platforms

All of Unity's platforms are supported. glTFast is tested or was reported to run on:

- WebGL
- iOS
- Android
- Windows
- macOS
- Linux
- Universal Windows Platform
- Lumin (Magic Leap)

## Workflows

|          | Runtime | Editor (design-time)
|----------| ------ | ------
| | |
| **GameObject**
| Import   | ✅️ | ✅
| Export   | [ℹ️][RuntimeExport] | <sup>1</sup> ☑️
| | |
| **Entities (see [DOTS](#data-oriented-technology-stack))**
| Import   | [☑️](#data-oriented-technology-stack) | `n/a`
| Export   |  | `n/a`

<sup>1</sup>: glTF export currently only works on Unity 2020.2 or newer.

## Core glTF features

The glTF 2.0 specification is fully supported, with only a few minor remarks.

| | Import | Export
|------------| ------ | ------
| **Format**
|glTF (.gltf) | ✅ | ✅
|glTF-Binary (.glb) | ✅ | ✅
| | |
| **Buffer**
| External URIs | ✅ | ✅
| GLB main buffer | ✅ | ✅
| Embed buffers or textures (base-64 encoded within JSON) | ✅ |
| [meshoptimizer compression][MeshOpt] (via [package][MeshOptPkg])| ✅ |
| | |
| **Basics**
| Scenes | ✅ | ✅
| Node hierarchies | ✅ | ✅
| Cameras | ✅ | 
| | |
| **Images**
| PNG | ✅ | ✅
| Jpeg | ✅ | ✅
| KTX with Basis Universal compression (via [KtxUnity](https://github.com/atteneder/KtxUnity)) | ✅ | 
| | |
| **Texture sampler**
| Filtering  | ✅ with [limitations](#Known-issues) | 
| Wrap modes | ✅ | 
| | |
| **Materials Overview** (see [details](#materials-details))
| [Universal Render Pipeline (URP)][URP] | ✅ | 
| [High Definition Render Pipeline (HDRP)][HDRP] | ✅ | 
| Built-in Render Pipeline | ✅ | ☑️
| | |
| **Topologies / Primitive Types**
| TRIANGLES | ✅ | ✅
| POINTS | ✅ | ✅
| <sup>1</sup>LINES | ✅ | ✅
| LINE_STRIP | ✅ | ✅
| <sup>1</sup>LINE_LOOP | ✅ | ✅
| TRIANGLE_STRIP |  | 
| TRIANGLE_FAN |  | 
| Quads | `n/a` | ✅ via triangulation
| | |
| **Meshes**
| Positions | ✅ | ✅
| Normals | ✅ | ✅
| Tangents | ✅ | ✅
| Texture coordinates / UV sets | ✅ | `?`
| Three or more texture coordinates / UV sets | [issue][UVsets] | `?`
| Vertex colors | ✅ | `?`
| Draco mesh compression (via [DracoUnity](https://github.com/atteneder/DracoUnity)) | ✅ | 
| Implicit (no) indices | ✅ | 
| Per primitive material | ✅ | ✅
| Joints (up to 4 per vertex) | ✅ | 
| Weights (up to 4 per vertex) | ✅ | 
| | |
| **Morph Targets / Blend Shapes**
| Sparse accessors | <sup>2</sup> ✅ | 
| [Skins][Skins] (sponsored by [Embibe](https://www.embibe.com)) | ✅ | 
| | |
| **Animation** 
| via legacy Animation System | ✅ | 
| via Playable API ([issue][AnimationPlayables]) |  | 
| via Mecanim ([issue][AnimationMecanim]) |  | 

<sup>1</sup>: Untested due to lack of demo files.

<sup>2</sup>: Not on all accessor types; morph targets and vertex positions only

## Extensions

### Official Khronos extensions

| | Import | Export
|------------| ------ | ------
| | |
| **Khronos**
| KHR_draco_mesh_compression | ✅ | 
| KHR_materials_pbrSpecularGlossiness | ✅ | 
| KHR_materials_unlit | ✅ | ✅
| KHR_texture_transform | ✅ | ✅
| KHR_mesh_quantization | ✅ | 
| KHR_texture_basisu | ✅ | 
| KHR_lights_punctual | [ℹ️][PointLights] | 
| KHR_materials_clearcoat | [ℹ️][ClearCoat] | 
| KHR_materials_sheen | [ℹ️][Sheen] | 
| KHR_materials_transmission | [ℹ️][Transmission] | 
| KHR_materials_variants | [ℹ️][Variants] | 
| KHR_materials_ior | [ℹ️][IOR] | 
| KHR_materials_specular | [ℹ️][Specular] | 
| KHR_materials_volume | [ℹ️][Volume] | 
| KHR_xmp |️ |
| | |
| **Vendor**
| <sup>1</sup>EXT_mesh_gpu_instancing | ✅ | 
| EXT_meshopt_compression | ✅ | 
| EXT_lights_image_based | [ℹ️][IBL] | 

<sup>1</sup>: Without support for custom vertex attributes (e.g. `_ID`)

Not investigated yet:

- AGI_articulations
- AGI_stk_metadata
- CESIUM_primitive_outline
- MSFT_lod
- MSFT_packing_normalRoughnessMetallic
- MSFT_packing_occlusionRoughnessMetallic

Will not be supported:

- KHR_techniques_webgl
- ADOBE_materials_clearcoat_specular (prefer KHR_materials_clearcoat)
- ADOBE_materials_thin_transparency (prefer KHR_materials_transmission)
- EXT_texture_webp (prefer KTX/basisu)
- FB_geometry_metadata (prefer KTX_xmp)
- MSFT_texture_dds (prefer KTX/basisu)

## Materials Details

### Material Import

| Material Feature              | URP | HDRP | Built-In |
|-------------------------------|-----|------|----------|
| PBR<sup>1</sup> Metallic-Roughness        | ✅  | ✅   | ✅       |
| PBR<sup>1</sup> Specular-Glossiness       | ✅  | ✅   | ✅       |
| Unlit                         | ✅  | ✅   | ✅       |
| Normal texture                | ✅  | ✅   | ✅       |
| Occlusion texture             | ✅  | ✅   | ✅       |
| Emission texture              | ✅  | ✅   | ✅       |
| Alpha modes OPAQUE/MASK/BLEND | ✅  | ✅   | ✅       |
| Double sided / Two sided      | ✅  | ✅   | ✅       |
| Vertex colors                 | ✅  | ✅   | ✅       |
| Multiple UV sets              | ✅<sup>2</sup>  | ✅<sup>2</sup>   | ✅<sup>2</sup>       |
| Texture Transform             | ✅  | ✅   | ✅       |
| Clear coat                    | [ℹ️][ClearCoat] | [ℹ️][ClearCoat] | [⛔️][ClearCoat] |
| Sheen                         | [ℹ️][Sheen] | [ℹ️][Sheen] | [⛔️][Sheen] |
| Transmission                  | [☑️][Transmission]<sup>3</sup> | [☑️][Transmission]<sup>4</sup> | [☑️][Transmission]<sup>4</sup> |
| Variants                      | [ℹ️][Variants] | [ℹ️][Variants] | [ℹ️][Variants] |
| IOR                           | [ℹ️][IOR]      | [ℹ️][IOR]      | [⛔️][IOR]      |
| Specular                      | [ℹ️][Specular] | [ℹ️][Specular] | [⛔️][Specular] |
| Volume                        | [ℹ️][Volume]   | [ℹ️][Volume]   | [⛔️][Volume]   |

<sup>1</sup>: Physically-Based Rendering (PBR) material model

<sup>2</sup>: Two sets of texture coordinates (as required by the glTF 2.0 specification) are supported, but not three or more ([issue][UVSets])

<sup>3</sup>: There are two approximation implementations for transmission in Universal render pipeline. If the Opaque Texture is enabled (in the Universal RP Asset settings), it is sampled to provide proper transmissive filtering. The downside of this approach is transparent objects are not rendered on top of each other. If the opaque texture is not available, the common approximation (see <sup>4</sup> below) is used.

<sup>4</sup>: Transmission in Built-In and HD render pipeline does not support transmission textures and is only 100% correct in certain cases like clear glass (100% transmission, white base color). Otherwise it's an approximation.

### Material Export



| Material Feature              | URP<sup>1</sup> | HDRP<sup>2</sup> | Built-In<sup>3</sup> |
|-------------------------------|-----|------|----------|
| PBR Metallic-Roughness        | `?` | `?` | ✅ |
| PBR Specular-Glossiness       |  |  |  |
| Unlit                         | `?` | `?` | ✅ |
| Normal texture                | `?` | `?` | ✅ |
| Occlusion texture             | `?` | `?` |  |
| Emission texture              | `?` | `?` |  |
| Alpha modes OPAQUE/MASK/BLEND | `?` | `?` | ✅ |
| Double sided / Two sided      | `?` | `?` | ✅ |
| Vertex colors                 | `?` | `?` | `?` |
| Multiple UV sets              | `?` | `?` | `?` |
| Texture Transform             | ✅ | ✅ | ✅ |
| Clear coat                    |  |  | `n/a` |
| Sheen                         | `?` | `?` | `n/a` |
| Transmission                  |  |  | `n/a` |
| Variants                      |  |  |  |
| IOR                           |  |  | `n/a` |
| Specular                      |  |  |  |
| Volume                        |  |  | `n/a` |

<sup>1</sup>: Universal Render Pipeline Lit Shader

<sup>2</sup>: High Definition Render Pipeline Lit Shader

<sup>3</sup>: Built-In Render Pipeline Standard and Unlit Shader

## Data-Oriented Technology Stack

> ⚠️ Note: DOTS is highly experimental and many features don't work yet. Do not use it for production ready projects!

Unity's [Data-Oriented Technology Stack (DOTS)][DOTS] allows users to create high performance gameplay. glTFast has experimental import support for it.

Instead of traditional GameObjects, glTFast will instantiate Entities with Hybrid Renderer (version 2) components.

Possibly incomplete list of things that are known to not work with Entities yet:

- Animation
- Skinning
- Morph targets
- Cameras

### DOTS Setup

- First, go through the official [DOTS project setup](https://docs.unity3d.com/Packages/com.unity.entities@0.17/manual/install_setup.html)
- Make sure to enable [Hybrid Renderer V2](https://docs.unity3d.com/Packages/com.unity.rendering.hybrid@0.11/manual/creating-a-new-hybrid-renderer-project.html)
- Use `GltfEntityAsset` instead of `GltfAsset`
- For customized behavior, use the `EntityInstantiator` instead of the `GameObjectInstantiator`

## Known issues

- <sup>1</sup>Vertex accessors (positions, normals, etc.) that are used across meshes are duplicated and result in higher memory usage and slower loading (see [this comment](https://github.com/atteneder/glTFast/issues/52#issuecomment-583837852))
- <sup>1</sup>When using more than one sampler on one image, that image is duplicated and results in higher memory usage
- Texture sampler minification/magnification filter limitations (see [issue][SamplerFilter]):
  - <sup>1</sup>There's no differentiation between `minFilter` and `magFilter`. `minFilter` settings are prioritized.
  - <sup>1</sup>`minFilter` mode `NEAREST_MIPMAP_LINEAR` is not supported and will result in `NEAREST`.

<sup>1</sup>: A Unity API limitation.

[AnimationMecanim]: https://github.com/atteneder/glTFast/issues/167
[AnimationPlayables]: https://github.com/atteneder/glTFast/issues/166  
[ClearCoat]: https://github.com/atteneder/glTFast/issues/68
[DOTS]: https://unity.com/dots
[HDRP]: https://unity.com/srp/High-Definition-Render-Pipeline
[IBL]: https://github.com/atteneder/glTFast/issues/108
[IOR]: https://github.com/atteneder/glTFast/issues/207
[MeshOpt]: https://github.com/KhronosGroup/glTF/tree/main/extensions/2.0/Vendor/EXT_meshopt_compression
[MeshOptPkg]: https://docs.unity3d.com/Packages/com.unity.meshopt.decompress@0.1/manual/index.html
[newIssue]: https://github.com/atteneder/glTFast/issues/new
[PointLights]: https://github.com/atteneder/glTFast/issues/17
[RuntimeExport]: https://github.com/atteneder/glTFast/issues/259
[SamplerFilter]: https://github.com/atteneder/glTFast/issues/61 
[Sheen]: https://github.com/atteneder/glTFast/issues/110
[Skins]: https://www.khronos.org/registry/glTF/specs/2.0/glTF-2.0.html#skins
[Specular]: https://github.com/atteneder/glTFast/issues/208
[Transmission]: https://github.com/atteneder/glTFast/issues/111
[URP]: https://unity.com/srp/universal-render-pipeline
[UVsets]: https://github.com/atteneder/glTFast/issues/206
[Variants]: https://github.com/atteneder/glTFast/issues/112
[Volume]: https://github.com/atteneder/glTFast/issues/209
