# FAQ

??? faq "Is MCUT restricted to triangle meshes?"
    No. MCUT is designed to work with meshes that are made with arbitrary--but planar--polygons. These polygons can be convex or convex, so triangle meshes are just a special case.

??? faq "Is it possible that MCUT can fail to fill holes in sliced objects?"
    MCUT can never fail to fill holes because the output meshes are produced by resolving/tracing the halfedge connectivity of input meshes. This is different from [winding numbers](https://igl.ethz.ch/projects/winding-number/robust-inside-outside-segmentation-using-generalized-winding-numbers-siggraph-2013-jacobson-et-al.pdf) and improves overall robustness since numerical operations are performed _only_ when resolving polygon intersections. 

??? faq "Does MCUT do automatic triangulation after cutting e.g. for rendering?"
    No. Polygon/mesh triangulation is reserved for specialised tools and libraries that are separate from MCUT. 

??? faq "Can MCUT cut more than two meshes at the same time?"
    MCUT is designed to work at-most with two meshes at a time. This simplifies reasoning about the implementation to reduce redundant complexity and simplify the code.

    You can use MCUT as a building block for more complex/nested cutting operations e.g. CSG trees. 

??? faq "I wish to slice a tetrahedral/hexahedral mesh using MCUT - is this possible?"
    In general, no. This is because tetrahedral/hexahedral meshes are non-manifold, which is beyond the scope of MCUT.

    However, you may be able to use MCUT as a building block to cut individual cells (tetrahedra/hexahedra) of your mesh with the given cutting surface. This of course implies that you must then resolve the connectivity of your non-manifold mesh manually, effectively building on top of MCUT.

??? faq "Can the cut surface be a non-manifold mesh?"
    No. The cut surface must always be a manifold surface mesh. 

??? faq "What constitues a valid mesh for MCUT?"
    A valid mesh is a 1) connected component (every face shares at least one edge with another) that is 2) a manifold surface (each edge is incident to one or two faces), and 3) contains no self-intersections which involve polygons _along the cut_.

??? faq "Where can I find the documentation for using MCUT?"
    Function documentation is provided in the main header file, which is `include/mcut/mcut.h`. There are three options for viewing this documentation: 1) building the docs with doxygen (see [Compilation](building)), 2) doing a Ctrl+F on `mcut.h` in your favourite text editor/IDE, and 3) using the github search function in the MCUT [repository](https://github.com/cutdigital/mcut.git).

    The [tutorials](tutorials/helloWorld/) are also useful.