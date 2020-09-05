# Caveats

# File formats

For simplicity, the executables `mcut_view` and `mcut_cmd` only support the object file format (OFF), i.e. `.off` files. This is because OFF is one of the simplest formats to specify mesh vertices and faces as a basic text file, which is all that MCUT requires. 

# Input meshes

## relative positioning

Any two input meshes must be intersect, otherwise there will be no resulting fragments or patches.

## A valid mesh

* 2-manifold
* self intersections
* consistent winding order (CW or CCW)