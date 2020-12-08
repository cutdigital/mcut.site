# The product

MCUT is distributed as a cross-platform software library (static or shared) exposing a classic C API which can be used in practically any C++ application.

The primary function of MCUT is to take two meshes, resolve their intersection(s) and generate fragment pieces of the sliced shape. When a mesh is cut, your application simply invokes a dispatch call. The MCUT API can then be queried for the desired output, like the patches that were used for hole-filling etc.

MCUT cuts with any 2-manifold surface. This has the advantages that it easy to slice a multitude of shapes - from using simple surfaces like quads to performing complex CSG operations using meshes of arbitrary shape. Furthermore, the cut mesh (your knife tool) does not depend on the complexity of the source mesh, thus alleviating numerous impinging constrants on what you can do with MCUT.

The technical details about how the MCUT algorithm works are discussed in a Journal article by Floyd M. Chitalu [here](https://onlinelibrary.wiley.com/journal/14678659).

----

<div>
  <img src="../media/mcut_armadillo-slice.png" alt="mcut-extremely-concave-cut" style="width:50%" class="center"/> 
  <p style="text-align:center;font-size:70%;">Armadillo sliced with planer cut.</p>
</div>

----