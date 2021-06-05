This tutorial walks through the basics of MCUT, showing how to define a mesh and iterate through its vertices and faces.

A mesh represents a two-manifold surface, where each edge is incident to one or two faces.

<div>
    <img src="../../media/simple-halfedge-mesh.png" alt="mcut-extremely-concave-cut" style="width:50%" class="center"/> 
    <p style="text-align:center;font-size:70%;">An example of a 2-manifold mesh. </p>
</div>

Meshes in MCUT are represented as simple arrays of builtin C/C++ data types. In particular, users are permitted to use three C/C++ types when specifying vertex coordinates.  These are `float` and `double`.

## 32-bit `float` vertex arrays 

 The following is an example of how to define a mesh using the `float` data type to represent vertex positions:

```cpp
float pMeshVertices[] = { // xyz|xyz|x...
  -10.f, -5.f, 0.f, // vertex 0
  -10.f, 5.f, 0.f, // vertex 1
  10.f, 5.f, 0.f,
  10.f, -5.f, 0.f,
  5.f, 0.f, 0.f,
  0.f, -5.f, 0.f,
  -5.f, 0.f, 0.f,
  -10.f, -10.f, 0.f,
  10.f, -10.f, 0.f,
  15.f, 5.f, 0.f,
  15.f, -10.f, 0.f
};
uint32_t pMeshFaceIndices[] = {
  0, 6, 5, 4, 3, 2, 1, // face 0
  0, 7, 8, 3, 4, 5, 6, // face 1
  8, 10, 9, 2, 3 // face 2
};
McFaceSize pMeshFaceSizes[] = {7, 7, 5};
uint32_t numMeshVertices = 11;
uint32_t numMeshFaces = 3;
``` 

## 64-bit `double` vertex arrays 

Using the same mesh example, the following except shows how to specify vertices as an array of `double`:

```cpp
double pMeshVertices[] = { 
  -10.0, -5.0, 0.0, 
  -10.0, 5.0, 0.0, 
  10.0, 5.0, 0.0,
  10.0, -5.0, 0.0,
  5.0, 0.0, 0.0,
  0.0, -5.0, 0.0,
  -5.0, 0.0, 0.0,
  -10.0, -10.0, 0.0,
  10.0, -10.0, 0.0,
  15.0, 5.0, 0.0,
  15.0, -10.0, 0.0
};
``` 

### Accessing mesh elements
  
As a simple demonstration of the mesh data structure defined above, let us iterate through the vertices and faces of the mesh. 

```cpp 
// vertices
for (uint32_t i = 0; i < numMeshVertices; ++i) {
  // assuming float
  float x = pMeshVertices[i*3 +0];
  float y = pMeshVertices[i*3 +1];
  float z = pMeshVertices[i*3 +2];

  printf("vertex: %f %f %f\n", x, y, z);
}

// faces
uint32_t base = 0;
for(int i = 0; i < numMeshFaces; ++i){
  uint32_t numFaceVertices = pMeshFaceSizes[i];
  printf("face %d (%hu vertices):", i, numFaceVertices);
  for(uint32_t j = 0; j < numFaceVertices; ++j)  {
    printf(" %u", pMeshFaceIndices[base+j]);
  }
  printf("\n");
  base += numFaceVertices;
}
```

