# Debugging

Using MCUT can be a lot of fun, but it can also be a large source of frustration whenever something isn't working just right, or perhaps not even cutting at all! Seeing as most of what we do involves manipulating meshes, it can be difficult to figure out the cause of error whenever something doesn't work. 

In this section we look into several ways of debugging your MCUT program, which is not too difficult to do.

## Error codes

All MCUT API functions return either an error code. The error codes that functions can return are listed below:

|     Flag       | Description      |
|:----------------|----------------|
|      MC_NO_ERROR          |         No user error reported |
|         MC_INVALID_SRC_MESH | The source-mesh is not two-manifold mesh         |
|           MC_INVALID_CUT_MESH | The cut-mesh is not two-manifold mesh            |
|   MC_INVALID_OPERATION | The state for a command is not legal for its given parameters            |
|     MC_INVALID_VALUE | Set when a value parameter is not legal.            |

Within MCUT's function documentation you can always find the error codes a function generates the moment it is incorrectly used. 

The great thing about returned error code is that they makes it relatively easy to pinpoint where any error may be and to validate the proper use of MCUT. Let's say you query zero connected components after an `mcDispatch` call and you have no idea what's causing it: did the meshes intersect? Where the input meshes watertight? By checking returned error flag all over your codebase, you can quickly catch the first place an MCUT error starts showing up.

The returned error codes are just numbers, which isn't easy to understand unless you've memorized the error codes. It often makes sense to write a small helper function to easily print out the error strings together with where the error check function was called:

```c++
void mcCheckError_(McResult err, const char *file, int line)
{
    std::string error;
    switch (err)
    {
        case MC_INVALID_SRC_MESH:              error = "MC_INVALID_SRC_MESH"; break;
        case MC_INVALID_CUT_MESH:              error = "MC_INVALID_CUT_MESH"; break;
        case MC_INVALID_VALUE:                 error = "MC_INVALID_VALUE"; break;
        case MC_INVALID_OPERATION:             error = "MC_INVALID_OPERATION"; break;
    }
    std::cout << error << " | " << file << " (" << line << ")" << std::endl;
}

#define mcCheckError(errCode) mcCheckError_(errCode, __FILE__, __LINE__) 
```

We can use this function as follows:
```c++
McResult err = mcGetConnectedComponents(
    context, 
    MC_TRUE, 
    MC_CONNECTED_COMPONENT_TYPE_ALL, 
    0, 
    NULL, 
    &numConnComps, 
    0, 
    NULL, 
    NULL);
mcCheckError(err);
```

Return codes don't help you too much because information that functions will return is rather simple. However, it does often help you catch typos or quickly pinpoint where things went wrong.

## Debug output

A more comprehensive tool is the _debug output_ feature. With the debug output, MCUT itself will directly send an error or warning message to the user with a lot more details compared to returned codes. Not only does it provide more information, it can also help you catch errors exactly where they occur by intelligently using a debugger.

To start use debug output we have to setup a debug output context. This step is a matter of specifying a flag:

```c++
McContext context;
McResult err = mcCreateContext(&context, MC_DEBUG);
// ...
```

To check if we successfully initialized a debug context we can query MCUT:


```c++
uint64_t numBytes = 0;
McFlags contextFlags;
err = mcGetInfo(context, MC_CONTEXT_FLAGS, 0, nullptr, &numBytes);
mcCheckError(err);

err = mcGetInfo(context, MC_CONTEXT_FLAGS, numBytes, &contextFlags, nullptr);
mcCheckError(err);

if (contextFlags & MC_DEBUG) {
    // ...
}
```

The way debug output works is that we pass MCUT an error logging callback function which we used to process the MCUT error data as we wish. For our example, we'll be displaying useful error data to the console. Below is the callback function prototype that MCUT expects for debug output:

```c++
void MCAPI_CALL (*pfn_mcDebugOutput_CALLBACK)(
    McDebugSource source, 
    McDebugType type, 
    uint32_t id, 
    McDebugSeverity severity, 
    size_t length, 
    const char* message, 
    const void* userParam);
```

We can then create a useful error printing utility function like:

```c++
void MCAPI_CALL mcDebugOutput(McDebugSource source,
    McDebugType type,
    unsigned int id,
    McDebugSeverity severity,
    size_t length,
    const char* message,
    const void* userParam)
{
    printf("---------------\n");
    printf("Debug message ( %d ): %s ", id, message);

    switch (source) {
    case MC_DEBUG_SOURCE_API:
        printf("Source: API");
        break;
    case MC_DEBUG_SOURCE_KERNEL:
        printf("Source: Kernel");
        break;
        break;
    }

    printf("\n");

    switch (type) {
    case MC_DEBUG_TYPE_ERROR:
        printf("Type: Error");
        break;
    case MC_DEBUG_TYPE_DEPRECATED_BEHAVIOR:
        printf("Type: Deprecated Behaviour");
        break;
    case MC_DEBUG_TYPE_OTHER:
        printf("Type: Other");
        break;
    }

    printf("\n");

    switch (severity) {
    case MC_DEBUG_SEVERITY_HIGH:
        printf("Severity: high");
        break;
    case MC_DEBUG_SEVERITY_MIDIUM:
        printf("Severity: medium");
        break;
    case MC_DEBUG_SEVERITY_LOW:
        printf("Severity: low");
        break;
    case MC_DEBUG_SEVERITY_NOTIFICATION:
        printf("Severity: notification");
        break;
    }

    printf("\n\n");
}
```

Whenever debug output detects an MCUT error, it will call this callback function and we'll be able to print out a large deal of information regarding the MCUT error. 

Now that we have the callback function it's time to initialize debug output:

```c++
if (contextFlags & MC_DEBUG) {
    mcDebugMessageCallback(context, mcDebugOutput, nullptr);
    mcDebugMessageControl(
        context, 
        MC_DEBUG_SOURCE_ALL, 
        MC_DEBUG_TYPE_ALL, 
        MC_DEBUG_SEVERITY_ALL, 
        true);
}
```

## Filter debug output

With `mcDebugMessageControl` you can filter the type(s) of errors you'd like to receive a message from. In our case we decided to not filter on any of the sources, types, or severity rates. If we wanted to only show messages from the MCUT API, that are errors, and have a high severity, we'd configure it as follows:

```c++
mcDebugMessageControl(
    context, 
    MC_DEBUG_SOURCE_API, 
    MC_DEBUG_TYPE_ERROR, 
    MC_DEBUG_SEVERITY_HIGH, 
    true);
```

Given our configuration, and assuming you have a context that supports debug output, every incorrect MCUT command will now print a bunch of potentially useful data.

## Backtracking the debug error source

Another great trick with debug output is that you can relatively easy figure out the exact line or call an error occurred. By setting a breakpoint in `mcDebugOutput` at a specific error type (or at the top of the function if you don't care), the debugger will catch the error thrown and you can move up the call stack to whatever function caused the message dispatch.

## Querying verbose runtime log

TODO