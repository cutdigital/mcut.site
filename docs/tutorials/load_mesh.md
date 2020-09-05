This tutorial walks through the basics of MCUT, showing how to define a mesh and iterate through its vertices and faces.

A mesh represents a very two-manifold surface, where each edge is incident to one or two faces.

TODO: add image of a two-manifold mesh


## Vertex arrays 

Meshes in MCUT are represented as simple arrays of builtin C/C++ data types. The following is an example of how to define a mesh using the `float` data type to represent vertex positions:

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

The mesh defined in the above code is shown in the following image:

TODO: add image of mesh


###  Using the `double` data type

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

### Using arbitrary precision 

If the `double` data type is insufficient, then arbitrary precision numbers maybe be used as follows:

```cpp
char pMeshVertices[] = { 
  "-10.0 -5.0 0.0\n"
  "-10.0 5.0 0.0\n"
  "10.0 5.0 0.0\n"
  "10.0 -5.0 0.0\n"
  "5.0 0.0 0.0\n"
  "0.0 -5.0 0.0\n"
  "-5.0 0.0 0.0\n"
  "-10.0 -10.0 0.0\n"
  "10.0 -10.0 0.0\n"
  "15.0 5.0 0.0\n"
  "15.0 -10.0 0.0"
};
``` 

... or


```cpp
char pMeshVertices[] = { 
  "-10.0 -5.0 0.0 -10.0 5.0 0.0 10.0 5.0 0.0 10.0 -5.0 0. 5.0 0.0 0.0 0.0 -5.0 0.0 -5.0 0.0 0.0 -10.0 -10.0 0.0 10.0 -10.0 0.0 15.0 5.0 0.0 15.0 -10.0 0.0"
};
``` 

Arbitrary precision numbers are specified as a character string where individual values are separated by spaces. To aide readability, tuples of three numbers can also be separated by a newline character. 

### Traversing the mesh
  
As a simple demonstration of the mesh data structure defined above, lets iterate through the vertices and faces of the mesh. 
```cpp 
// vertices
for (int i = 0; i < numMeshVertices; ++i) {
  // assuming float
  float x = pMeshVertices[i*3 +0];
  float y = pMeshVertices[i*3 +1];
  float z = pMeshVertices[i*3 +2];

  printf("vertex: %f %f %f\n", x, y, z);
}

// faces
int off = 0;
for(int i = 0; i < numMeshFaces; ++i)
{
  McFaceSize numFaceVertices = pMeshFaceSizes[i];
  
  printf("face %d (%hu vertices): ", i, numFaceVertices);
  
  for(int j = 0; j < numFaceVertices; ++j)
  {
    printf(" %u", pMeshFaceIndices[off+j]);
  }

  printf("\n");

  off += numFaceVertices;
}
```

