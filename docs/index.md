# Home

<div class="container">
    <div style="float:left;width:100%">
	   <img src="media/repo-teaser/website-teaser.png" alt="bunny1" />
    </div>
</div>

**MCUT** (pronounced 'emcut') is a tool for cutting meshes: It is a library for partitioning meshes that are two-manifold and is useful for operations like slicing and CSG boolean operations. 

The library partitions shapes directly from input mesh data composed of geometry and connectivity to produce crisp fragments at fine scale. It is the only existing tool allowing cuts on planar-polygon meshes without requiring them to be triangulated solids.  

MCUT also provides features like stencilling to produce cut-outs/patches of the cutting surface that is used to partition a given shape. It also supports intersection-curve queries and partial cuts, using one simple interface.

## Motivation

Mesh cutting is fundamental and useful for solving a wide set of problems. The goal is to split a given surface mesh into a set of disjoint parts using another surface mesh.
These resulting parts are typically employed in model design and/or simulation, such as virtual surgery, game level-design, computer aided design and manufacturing.

Despite existing tools (e.g. [CGAL](https://www.cgal.org/), [Cork](https://github.com/gilbo/cork), [Carve](https://code.google.com/archive/p/carve/), [PWN](http://www.cs.columbia.edu/cg/mesh-arrangements/#definitions) or [TetMesh tools](https://github.com/loopstring/3d-cutter.git)), it remains a challenge to slice meshes without restrictive assumptions on the input. Moreover, besides traditional CSG operations, practically all sophisticated modelling software (like [Maya](https://www.autodesk.com/products/maya/overview), [Cinema4D](https://www.maxon.net/en/cinema-4d), [Blender](https://www.blender.org/), [MeshMixer](https://www.meshmixer.com/), [ANSYS SpaceClaim](https://www.ansys.com/products/3d-design/ansys-spaceclaim) etc.) allow users only to cut meshes with a plane which restricts modelling and design capabilities.

## Features

In addition to being simple, fast and robust, MCUT is specifically designed to be _a cutting tool_ which supports the following features: 

* **Manifold meshes**: _open_  (with borders/boundaries), or _closed_ (as in 'watertight'). 
* **Partial cuts**: A sliced object need not be completely cut into disjoint parts. 
* **Stencilling**: Silhouette cut-outs of the cutting surface patches.
* **Intersection curves**: Vertices/edges introduced as a result of the cut.
* **Booleans**: Traditional CSG operations. 
* **N-gons**: Arbitrary planar-polygon subdivisions. 

### Example 

![Teaser](media/mcut-cube-cut.png)

The above image shows a simple example of what MCUT can do. On the left is a cube (the "source mesh") that is cut by a circular surface (the "cut mesh"), which together comprise the _input_. On the right is the resulting set of connected components after partitioning the cube. In general, the _output_ of MCUT includes unsealed fragments (mid-left), cut mesh patches (middle), and the sealed fragments whose holes have been filled with cut mesh polygons that lie on the interior of the source mesh. Sealing can also be done using cut mesh polygons that lie on the exterior of the source mesh. 

