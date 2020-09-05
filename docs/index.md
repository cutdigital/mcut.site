# Welcome to MCUT 

<div class="container">
    <div style="float:left;width:49%">
	   <img src="media/mcut_bunny0.jpg" alt="bunny1" />
    </div>
    <div style="float:right;width:49%">
	    <img src="media/mcut_bunny1.jpg" alt="bunny11" />
    </div>
</div>

## Overview

MCUT is a tool for cutting meshes. It is a library for partitioning 2-manifold polygon meshes in order to perform operations like _slicing_, _stenciling_, _boolean operations_ and more! 

MCUT partitions shapes as meshes directly from input data composed of geometry and connectivity to produce crisp fragments at fine scale. There are no precomputed tetrahedralizations or signed distance fields (voxel level sets). MCUT uses a [breakthrough procedure](https://onlinelibrary.wiley.com/doi/abs/10.1111/cgf.13953) that we invented to achieve robustness with reliability, and it is the only existing method that properly cuts arbitrary-planar polygon meshes with no requirement for triangulation for meshes.

MCUT also provides stencilling services that calculate the exact cut-outs of the cutting surface which is used to partition a shape. In addition to basic slicing and hole filling, it can perform intersection-path queries, constructive solid geometry (CSG) operations, partial cuts, and surface-to-surface cuts.

## Why mesh cutting?

Cutting is a fundamental computational geometry problem whose solution is useful in a wide set of application domains. The goal of cutting is to partition a given surface mesh, described by its vertices and connectivity, into a set of disjoint parts.
These resulting parts are typically employed for further model design and/or simulation, such as virtual surgery, computer aided design, and simulation such as fracture.

Despite existing tools (e.g. [CGAL](https://www.cgal.org/), [Cork](https://github.com/gilbo/cork), [Carve](https://code.google.com/archive/p/carve/), or [tetrahedral-mesh tools](https://github.com/loopstring/3d-cutter.git)), it is still a challenge to cut arbitary manifold surfaces without restrictive assumptions on the input meshes. 

## What MCUT can do

![Teaser](media/mcut-cube-cut.png)

The above image shows a simple example of what MCUT can do. On the left is a cube (the "source mesh") that is cut by a circular surface (the "cut mesh"), which together comprise the _input_. On the right is the resulting set of connected components after partitioning the cube. In general, the _output_ of MCUT includes unsealed fragments (mid-left), cut mesh patches (middle), and the sealed fragments (partially or completely) whose holes have been filled with cut mesh polygons that lie on the interior of the source mesh. Sealing can also be done using cut mesh polygons that lie on the exterior of the source mesh. 

<div class="row">
  <div class="column">
    <img src="media/mcut-cube-cut-surface-partial.png" alt="drawing1" style="width:100%"/> 
    <p style="text-align:center;font-size:80%;">Input</p>
  </div>
  <div class="column">
    <img src="media/mcut-cube-stretched-partial-cut.png" alt="drawing3" style="width:100%"/>
    <p style="text-align:center;font-size:80%;">Result</p>
  </div>
  <div class="column">
    <img src="media/mcut-cube-partial-cut.png" alt="drawing2" style="width:100%"/> 
    <p style="text-align:center;font-size:80%;">Stretched</p>
  </div>
</div>

MCUT is a general tool in that it is particularly suited for incremental cuts by supporting N-gons. Input meshes can have _arbitrary planar-polygons_ (convex or concave), thus avoiding strict restrictions of triangulations which are not unique with subsequent cuts. In effect, the connectivity of the resulting fragments is identical to the (uncut) source mesh except at edges introduced by the cut. Furthermore, the cut mesh need not partition the source mesh completely to allow _partial cut intersections_ - a feature which further lessens constraints on the relative placement of the inputs w.r.t. one another.

<div>
    <img src="media/mcut-extremely-concave-cut.png" alt="mcut-extremely-concave-cut" style="width:50%" class="center"/> 
    <p style="text-align:center;font-size:70%;">An extreme example of N-gon mesh cutting. </p>
</div>

The image above shows an extreme example, which is a result of cutting a source mesh that has concave polygons. The source mesh was a pentagonal frustum with the pentagons (top and bottom faces) made concave (and not parallel to each other). Each pentagon was composed of polygons with several concavities. The whole model was composed of only one volume element (all edges are on the surface). MCUT produces the correct fragments, and does not modify the connectivity except where intersected with the cut mesh, which was a plane (quad).

<div>
  <img src="media/mcut-zombiehead-scene.png" alt="mcut-extremely-concave-cut" style="width:70%" class="center"/> 
  <p style="text-align:center;font-size:70%;">Jagged surface cut. </p>
</div>

MCUT is _robust_, relying on well-tested geometric predicates for resolving intersections. By default, numerical operations are computed exactly up to machine precision. If so desired however, the library can be configured to work with arbitrary-precision, surpassing limitations of machine precision for increased reliability and peace of mind. 

---