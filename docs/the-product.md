# The product

MCUT is distributed as a library (static or shared) that has a basic C interface (API), and full source is included with every license. MCUT is in general platform agnostic and runs on most platforms (Windows, Mac, Linux). It can be used in practically any C++ application. The API is fully [documented](https://github.com/cutdigital/mcut-docs).

The primary function of MCUT is to take two meshes (source mesh and cut mesh), resolve their intersections, and generate mesh fragments (connected components) of the partitioned source mesh. When a mesh is cut, your application simply invokes a dispatch call. The API can then be queried for the desired output connected components, like the cut mesh patches which were used for hole filling etc.

MCUT can cut with any 2-manifold surface. This has the advantage that its easy to clip shapes with simple surfaces like a quad, or perform CSG operations using meshes of arbitrary shape (convex or convex). It also means that the cut mesh does not depend on the complexity of the source mesh. There is also an ability to query intersection contours which are lists of vertex indices that lie along the paths of intersection between the source and cut mesh.

Upon request, MCUT also comes with a standalone command-line tool that reads meshes from file and generates new mesh files with the .off extension. The .off file is a well known text file format specifying mesh geometry and polygons which is simple and viewable using publicly available tools like [Blender](https://www.blender.org/).

The technical details about how the MCUT algorithm works are discussed in a paper by Floyd M. Chitalu in the [Computer Graphics Forum](https://onlinelibrary.wiley.com/journal/14678659).