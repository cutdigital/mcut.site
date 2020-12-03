# Design principals

Here we summarize the main design principles in MCUT's design as a user interface:

1. **Simplicity**: As a user, you need only use standard arrays for passing data to MCUT, which are simple fixed-size sequential collections of elements of the same type. We expose MCUT functionality with a familiar C-style API which greatly favors code simplicity and makes code portable (e.g. for binding).

2. **Minimal dependencies**: In general, MCUT has no dependancies. Users may however choose to use external libraries (in fact, just one which is MPFR) if they so desire, and we wrap them in a small set of configuration variables that can be set at build time (see [Compilation](building)).