# Debugging

Using MCUT can be a lot of fun, but it may also be a source of frustration whenever something isn't working just right! Since most of what you will do involves working with geometry, it may be difficult to figure out the source of an error whenever something does not work as expected. 

In this tutorial we look into several ways of debugging your MCUT program. 

## Error codes

All MCUT API functions return an error code. The error codes that functions can return are listed below:

|     Flag       | Description      |
|:----------------|----------------|
|      MC_NO_ERROR          |         No user error reported. |
|         MC_INVALID_SRC_MESH | The source-mesh is not two-manifold mesh.         |
|           MC_INVALID_CUT_MESH | The cut-mesh is not two-manifold mesh.            |
|   MC_INVALID_OPERATION | The state for a command is not legal for its given parameters.            |
|     MC_INVALID_VALUE | Set when a value parameter is not legal.            |
| MC_OUT_OF_MEMORY | Internal memory allocation failure.     |

Within MCUT's function documentation you can always find the error codes a function generates the moment it is incorrectly used. 

The great thing about returned error codes is that they makes it relatively easy to pinpoint where any error _may_ be and to validate the proper use of MCUT. Let's say you query zero connected components after an `mcDispatch` call and you have no idea what's causing it: did the meshes intersect? Where the input meshes valid? By checking returned error flag all over your codebase, you can quickly catch the first place an MCUT error starts showing up.

## Helper functions

The returned error codes are just numbers, which isn't easy to understand unless you've memorized the error codes. It often makes sense to write a small helper function to easily print out the error strings together with where the error check function was called:

```c++
void mcCheckError_(McResult err, const char *file, int line)
{
    if(err != MC_NO_ERROR){
        std::string errorStr;
        switch (err)
        {
            case MC_INVALID_SRC_MESH:               errorStr = "MC_INVALID_SRC_MESH"; break;
            case MC_INVALID_CUT_MESH:               errorStr = "MC_INVALID_CUT_MESH"; break;
            case MC_INVALID_VALUE:                  errorStr = "MC_INVALID_VALUE"; break;
            case MC_INVALID_OPERATION:              errorStr = "MC_INVALID_OPERATION"; break;
            case MC_OUT_OF_MEMORY:                  errorStr = "MC_OUT_OF_MEMORY"; break;
        }
        std::cout << errorStr << " | " << file << " (" << line << ")" << std::endl;
    }
}

#define mcCheckError(errCode) mcCheckError_(errCode, __FILE__, __LINE__) 
```

We can then use this function as follows:
```c++
// make some API call and return error code
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
    
// ... now check the error code
mcCheckError(err);
```

## Debug output

Helper functions are useful, but return codes alone can't help too much because information that functions will return is rather simple. However, it does often help you catch typos or quickly pinpoint where things went wrong.

A more comprehensive tool is the _debug output_ feature. With the debug output, MCUT will directly send an error or warning message to the user with a lot more details compared to returned codes. 

Not only does it provide more information, it can also help you catch errors exactly where they occur by intelligently using a debugger.

To start using debug output we have to setup a _debug context_. This step is a matter of specifying a flag:

```c++
McContext context = MC_NULL_HANDLE;
McResult err = mcCreateContext(&context, MC_DEBUG);

if(err)
{
    // ...
}
```

To check if we successfully initialized a debug context we can query MCUT as follows:


```c++
uint64_t numBytes = 0;
McFlags contextFlags;

McResult err = mcGetInfo(context, MC_CONTEXT_FLAGS, 0, nullptr, &numBytes);

if(err)
{
    // ...
}

err = mcGetInfo(context, MC_CONTEXT_FLAGS, numBytes, &contextFlags, nullptr);

if(err)
{
    // ...
}

if (contextFlags & MC_DEBUG) {
    // do stuff with our debug context...
}
```

The way debug output works is that we pass MCUT an _error logging callback function_. This callback function will be used to process the MCUT error data as we wish. For this particular example, we'll be displaying useful error data to the console. 

Here is the callback function prototype that MCUT expects for debug output:

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

We can then create a useful error printing utility function like this:

```c++
extern MCAPI_ATTR void MCAPI_CALL mcDebugOutput(McDebugSource source,
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

Now that we have the callback function, the next step is to initialize debug output:

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

## Filtering debug output

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

**Backtracking the debug error source**

Another great trick with debug output is that you can relatively easily figure out the exact line or call an error occurred. By setting a breakpoint in `mcDebugOutput` at a specific error type (or at the top of the function if you don't care), the debugger will catch the error thrown and you can move up the call stack to whatever function caused the message dispatch.

## Verbose runtime log

To supplement the above debugging methods, MCUT also allows users to query a detailed runtime log. This log details every step of execution up to the point of failure - that is if runtime did indeed fail, otherwise the log represents a clean pass. 

This feature is--in essence--a last-ditch attempt to pinpoint the source of error. The main use is for filing bug reports.

To query the detailed runtime log, we use the `mcGetInfo` function as follows:

```c++
std::vector<char> log;
uint64_t numBytes = 0;

McResult err = mcGetInfo(context, MC_DEBUG_KERNEL_TRACE, 0, nullptr, &numBytes);

if(err)
{
    // ...
}

log.resize(numBytes);

err = mcGetInfo(context, MC_DEBUG_KERNEL_TRACE, numBytes, &log[0], nullptr);

if(err)
{
    // ...
}
```

The data contained in `log` can then be printed or logged to file as required. 

Be aware that information queried with the flag `MC_DEBUG_KERNEL_TRACE` reflects execution state following the most-recent `mcDispatch` call.
