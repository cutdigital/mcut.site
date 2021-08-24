# Design principles

![Teaser](../media/mcut-cube-cut.png)

Here we summarize the design principles of MCUT:

* **Fast**: Processing meshes with over 100k polygons in a few seconds.

* **Robust**: Thorougly tested and works with exact predicates. 

* **Simple**: Straightforward and portable C-API.

* **Minimal**: Self-contained and without dependancies.

----

The above image depicts the gist of what MCUT does. On the left is a cube (the "source mesh") that is cut by a circular surface (the "cut mesh"), which together comprise the _input_. On the right is the resulting set of connected components after partitioning the cube. In general, the _output_ of MCUT includes unsealed fragments (mid-left), cut mesh patches (middle), and the sealed fragments whose holes have been filled with cut mesh polygons that lie on the interior of the source mesh. Sealing can also be done using cut mesh polygons that lie on the exterior of the source mesh. 

