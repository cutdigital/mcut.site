# Obtaining information about context state

MCUT uses several opaque data structures whose properties can be examined with
functions named `mcGetXX`. Context objects are no exception. The `mcGetInfo`
function provides information about a context object’s state,
and its signature is as follows

```c++
McResult mcGetInfo(
    const McContext context,
    McFlags info,
    uint64_t bytes,
    void* pMem,
    uint64_t* pNumBytes);
```

These arguments are straightforward. The first three provide input: the context
object, a name that identifies the type of data you’re requesting, and the amount of
data you’re requesting. The last two arguments are output arguments, in which the
function returns the data you’re requesting and the size of the returned data.

The data type of the fourth argument is dependant on the second argument. The second argument is an enumerated type that can take any of the values in the following table:

|     Parameter name       | Parameter value      | Purpose |
|:----------------|----------------|----------------|
| `MC_CONTEXT_FLAGS` | `McFlags` | Returns a bitfield encoding the flags used to create the context. |
| `MC_DEFAULT_PRECISION` | `uint64_t` | Default number of bits used to represent the significand of a floating-point number. |
| `MC_DEFAULT_ROUNDING_MODE` | `McRoundingModeFlags` | Default way to round the result of a floating-point operation. |
| `MC_PRECISION_MAX` | `uint64_t` | Maximum value for precision bits. |
| `MC_PRECISION_MIN` | `uint64_t` | Minimum value for precision bits. |


Here is an example of how to use the `mcGetInfo` function:

```c++
McFlags defaultRoundingMode = 0;
uint64_t numBytes = 0;
McResult err = mcGetInfo(context, MC_DEFAULT_ROUNDING_MODE, 0, nullptr, &numBytes);
if(err)
{
    // ...
}

err = mcGetInfo(context, MC_DEFAULT_ROUNDING_MODE, numBytes, &defaultRoundingMode, nullptr);

if(err)
{
    // ...
}

std::fprintf(stdout, "default precision: %" PRId64 "\n", (uint64_t)defaultPrec);
```

If MCUT has not been compiled with arbitrary-precision numbers, then information dependant on `MC_DEFAULT_PRECISION`, `MC_PRECISION_MAX` and `MC_PRECISION_MIN` should not be relied on. 