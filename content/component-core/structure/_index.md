+++
title = "Structure"
weight = 2
+++

There are several core pieces of the Realtime Mesh Component, that all work together.

## URealtimeMeshActor

The RealtimeMeshActor is much like the StaticMeshActor. It provides and easy base class for an RMC powered actor and provides some helper logic to not regenerate on construction script in editor when doing things like dragging the actor around. It doesn't provide much more logic on its own and you can ignore it and create the RealtimeMeshComponent directly on your own actor types, or even multiple RMCs on your own actor type.


## URealtimeMeshComponent

The RealtimeMeshComponent is the minimum usable component, and provides similar functionality to how StaticMeshComponent works where you can place one or more of them on an actor. 


## URealtimeMesh

The RealtimeMesh object holds the underlying data, manages the collision structurs, and common data, and allows sharing a RealtimeMesh with multiple RealtimeMeshComponents and thereby multiple RealtimeMeshActors or custom actors. There are a couple versions of a RealtimeMesh including RealtimeMeshSimple, RealtimeMeshDynamic, and RealtimeMeshComposable. You can also make your own versions of RealtimeMesh or derivatives of any of the other concrete implementations.

1. RealtimeMeshSimple - Provides a similar feel to ProceduralMesh, and older style Realtime/RuntimeMeshComponents where you can feed it data and forget about it.
2. RealtimeMeshDynamic(Pro) - Provides a way to feed a DynamicMesh into the RealtimeMesh to get the benefits of GeometryScripting in editor and Runtime, with the performance/feature benefits of RealtimeMesh
3. RealtimeMeshComposable(Pro) - Provides a way to work with a hierarchical factory structure to generate one or more meshes.


### FRealtimeMesh

FRealtimeMesh is the central piece that communicates with the rendering thread, and defines an interface that must be implemented by the concrete tyeps which provides the rendering and collision data when necessary.


### FRealtimeMeshMaterialSlot

Much like a material slot in a static mesh, defines the separate materials in use by the realtime mesh. A renderable section uses the index of the slot to choose the material it will ultimately use.


### FRealtimeMeshLOD

Starting the hierarchy of mesh data, it encapsulates an abritrary number of section groups/sections to render when the LOD is desired.

The configuration of a LOD contains the ScreenSize to activate this LOD, if a higher level LOD isn't active.

LODs are completely independent, and can contain different numbers and configurations of section groups and sections. With this, the more detailed LODs could contain more independent sections to provide more detail and use simpler combined sections in lower LODs for performance.


### FRealtimeMeshSectionGroup

The middle of the hierarchy of mesh data, it contains a set of vertex and index buffers that can be shared by an abitrary number of sections within the group.

Allows for re-using the buffer between sections, as well as minimizing state changes when rendering sections.


### FRealtimeMeshSection

The lowest level of th hierarchy of mesh data. It links a section of vertex/index buffers in the parent section group, with a material, and rendering settings to draw a portion of the mesh within the active LOD.

Sections can be used to render portions of the parent groups buffers in differnt ways. You could use this to render a different region of the index buffer for shadows using a subset of vertices, and another section to render the full index buffer for the more detailed visible mesh. 

The Stream Range of a section is the start/end element of the vertices and indices in the parent section groups buffers.

The Section Config allows setting multiple configuration options for the section including whether the section is visible, or casts a shadow, as well as the material slot. It also allows setting the draw type from one of the two following options:
1. Static - Meant for sections that don't update frequently, so it prioritizes rendering performance at the cost of slower updates
2. Dynamic - Meant for sections that update frequently even up to frame-by-frame, so it prioritizes update latency over rendering performance.


### FRealtimeMeshProxy

The render thread counterpart to FRealtimeMesh, owns all the RHI data and works with the renderer to render the Realtime Mesh.

This is done, like other render proxy objects in the engine, to own a separate state on the render thread so not to have lock contention with the game thread.


### FRealtimeMeshLODProxy

The render thread counterpart to FRealtimeMeshLOD, owns all the render thread section groups and sections used to render the Realtime Mesh


### FRealtimeMeshSectionGroupProxy

The render thread counterpart to FRealtimeMeshSectionGroup, owns all the render thread sections used to render the RealtimeMesh. Also owns the actual RHI buffers, vertex factories, and ray tracing data.


### FRealtimeMeshSectionProxy

The render thread counterpart to FRealtimeMeshSection, owns all the render thread configuration for the section, clones the configuration from the parent Section to allow the render thread to facilitate the renderer in setting up mesh drawing.


### FRealtimeMeshComponentProxy

The render thread counterpart to URealtimeMeshComponent, owns the render thread state of a URealtimeMeshComponent, including its linkage to the underlying FRealtimeMeshProxy to facilitate the component rendering.


