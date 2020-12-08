# Querying exact coordinates

As described in the [meshes tutorial](tutorials/meshes), mesh vertices that are represented using arbitrary precision will be specified using character strings (i.e. as `const char*`). A single string specifies the coordinates as a space-separated sequence of real numbers. (Tuples of three numbers are also possible, placing newline character in place of every third space character). 

With this format, this tutorial shows how to query/ask MCUT for vertex coordinates as a character string representing their exact values. 

The first step is querying the actual number of vertices (in a given connected component). We must explicitly query the number of vertices because this information cannot be inferred from the number of bytes - unlike `double` and `float`. 

```c++
uint64_t vertexCountBytes = 0;
McResult err = mcGetConnectedComponentData(
    context, 
    MC_TRUE, 
    cc, // given connected component 
    MC_CONNECTED_COMPONENT_DATA_VERTEX_COUNT, 
    0, 
    NULL, 
    &vertexCountBytes, 
    0, NULL, NULL);

if(err)
{
    // ...
}

// It possible to also directly query the number of vertices 
// since we know the number of bytes, which is a constant 4 bytes!
uint32_t numberOfVertices = 0;

McResult err = mcGetConnectedComponentData(
    context_, 
    MC_TRUE, 
    cc, 
    MC_CONNECTED_COMPONENT_DATA_VERTEX_COUNT, 
    vertexCountBytes, 
    &numberOfVertices, 
    NULL, 
    0, NULL, NULL);

if(err)
{
    // ...
}
```

Once we know the number of vertices, the next step is querying the exact coordinates which we do as follows:

```c++
uint64_t connCompVerticesBytes = 0;
McResult err = mcGetConnectedComponentData(context_, MC_TRUE, cc, MC_CONNECTED_COMPONENT_DATA_VERTEX_EXACT, 0, NULL, &connCompVerticesBytes, 0, NULL, NULL);

if(err)
{
    // ...
}

std::vector<char> rawVerticesString;
rawVerticesString.resize(connCompVerticesBytes);

McResult err = mcGetConnectedComponentData(context_, MC_TRUE, cc, MC_CONNECTED_COMPONENT_DATA_VERTEX_EXACT, connCompVerticesBytes, (void*)rawVerticesString.data(), NULL, 0, NULL, NULL);

if(err)
{
    // ...
}

auto isPartOfDigit = [&](unsigned char ch) {
    return ch == '.' || ch == '-' || std::isdigit(ch);
};

std::vector<char> x;
std::vector<char> y;
std::vector<char> z;

const char* vptr = reinterpret_cast<const char*>(rawVerticesString.data());
const char* vptr_ = vptr; // shifted

// for each number ...
for (uint32_t i = 0; i < numberOfVertices * 3; ++i) {

    vptr_ = std::strchr(vptr, ' '); // find next space

    // length of string representing a number (very long!)
    std::ptrdiff_t diff = vptr_ - vptr;
    uint64_t srcStrLen = diff + 1; // extra byte for null-char
    
    if (vptr_ == nullptr) {
        srcStrLen = std::strlen(vptr) + 1;
    }

    if ((i % 3) == 0) { // x coord
        x.resize(srcStrLen);
        std::sscanf(vptr, "%s", &x[0]);
        x.back() = '\0';

        if(!isPartOfDigit(x[0]))
        {
            // ...
        }
    } else if ((i % 3) - 1 == 0) { // y coord
        y.resize(srcStrLen);
        std::sscanf(vptr, "%s", &y[0]);
        y.back() = '\0';

        if(!isPartOfDigit(y[0]))
        {
            // ...
        }
    } else if ((i % 3) - 2 == 0) { // z coord
        z.resize(srcStrLen);
        std::sscanf(vptr, "%s", &z[0]);
        z.back() = '\0';

        if(!isPartOfDigit(z[0]))
        {
            // ...
        }
    }

    vptr = vptr_ + 1; // offset so that we point to the start of the next number/line
}
```

Note that using `MC_CONNECTED_COMPONENT_DATA_VERTEX_EXACT` guarrantees exact numbers only if the following is true 

1. `mcDispatch` was called with meshes defined using `MC_DISPATCH_VERTEX_ARRAY_EXACT`, and 
2. MCUT libary was compiled with arbitrary precision numbers enabled (see [Compilation](building)). 

Otherwise, the operation is equivalent to using `std::to_string` on values of type `float` or `double`, respectively.
