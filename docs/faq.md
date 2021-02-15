# FAQ

??? faq "Is MCUT restricted to triangle meshes?"
    No. MCUT is designed to work with meshes that are made with arbitrary--but planar--polygons. These polygons can be convex or convex, so triangle meshes are just a special case.

??? faq "Is it possible that MCUT can fail to fill holes in sliced objects?"
    MCUT can never fail to fill holes because the output meshes are produced by resolving/tracing the halfedge connectivity of input meshes. This is different from [winding numbers](https://igl.ethz.ch/projects/winding-number/robust-inside-outside-segmentation-using-generalized-winding-numbers-siggraph-2013-jacobson-et-al.pdf) and improves overall robustness since numerical operations are performed _only_ when resolving polygon intersections. 

??? faq "Does MCUT do automatic triangulation after cutting e.g. for rendering?"
    No. Polygon/mesh triangulation is reserved for specialised tools and libraries that are separate from MCUT. 

??? faq "Can MCUT cut more than two meshes at the same time?"
    MCUT is designed to work at-most with two meshes at a time. This simplifies reasoning about the implementation to reduce redundant complexity.

??? faq "I wish to slice a tetrahedral/hexahedral mesh using MCUT - is this possible?"
    In general, no. This is because tetrahedral/hexahedral meshes are non-manifold, which is beyond the scope of MCUT.

    However, you may be able to use MCUT as a building block to cut individual cells (tetrahedra/hexahedra) of your mesh with the given cutting surface. This of course implies that you must then resolve the connectivity of your non-manifold mesh manually, effectively building on top of MCUT.

??? faq "Can the cut surface be a non-manifold mesh?"
    No. The cut surface must always be a manifold surface mesh. 

??? faq "Handling edge-edge and face-vertex intersections"
    The aim of MCUT is to resolve the combinatorial structure of the meshes being cut, focusing strictly on topology/connectivity. Thus, MCUT expects that all polygon intersections can be reduced to edge-face intersections, which alleviates a heavy reliance on numerical operations through out the execution pipeline. Moreover MCUT will only use numerical (e.g. floating point) computations to find where polygons intersect. The remaining logic is a deterministic set of well defined algorithms to separate meshes as advertised.

??? faq "What constitues a valid mesh for MCUT?"
    A valid mesh is a connected component which is a manifold surface that has no self-intersections involving polygons _along the cut_. We take the definition of manifold to be a mesh where every edge is incident to (i.e. used by) one or two faces.

??? faq "Where can I find the documentation for using MCUT?"
    Function documentation is provided in the main header file, which is `include/mcut/mcut.h`. There are three options for viewing this documentation: 1) building the docs with doxygen (see [Compilation](building)), 2) doing a Ctrl+F on `mcut.h` in your favourite text editor/IDE, and 3) using the github search function in the MCUT [repository](https://github.com/cutdigital/mcut.git).

    The [tutorials](tutorials/helloWorld/) are also useful.

??? faq "What is a 'mesh'?"
    A mesh is a data structure consisting of a set of vertices, edges, faces and topological relations between them. This data structure defines how each element is stored.

??? faq "What is a 'connected component'?"
    A connected component is the general name given to a mesh. In particular, it has the property that for any two faces in this mesh, there is _a path of adjacent faces_ such that all edges between two consecutive faces of the path are not marked as constrained.

??? faq "What is a 'source mesh'?"
    A source mesh is the mesh you want to cut.

??? faq "What is a 'cut mesh'?"
    A cut mesh is the mesh you want to cut with (the knife tool).

??? faq "What is a 'fragment'?"
    A fragment is a connected component that resulted from partitioning the source mesh - sealed or unsealed.
    
??? faq "What is a 'patch'?"
    A patch is a connected component that is used to a seal hole in a fragment. It is a piece produced from the cut mesh.

??? faq "What is a 'seamed connected component'?"
    A connected component produced by MCUT which is same as an input mesh (source mesh or cut mesh) except that it has new edges which are placed according to the cut.  

??? faq "What is the meaning of 'fragment location'?"
    The relative location of a fragment with respect to the cut mesh, which is determined by the face orientation (winding order) of the cut mesh. 
    
    *Example*: A tetrahedron (source mesh) that is sliced in the middle with a quad (cut mesh), where the apex lies in the normal direction of the quad. In this case, the apex fragment is considered 'above', while the 6 sided fragment is considered 'below'.

??? faq "What is the meaning of 'patch location'?"
    The relative location of a patch with respect to the source mesh, which is determined by the face orientation (winding order) of the source mesh. Intuitively, the subset of cut mesh polygons--after clipping--that lie inside (say, a watertight) source mesh are considered 'inside', and those not as 'outside'. 

??? faq "Must all input meshes have a consistent winding order?"
    Yes. All input meshes must have a consistent winding order, but this order can be either clock-wise (CW) or counter clock-wise (CCW). Note however, _that the source mesh can be CCW while the cut mesh is CW, and vice-versa_.