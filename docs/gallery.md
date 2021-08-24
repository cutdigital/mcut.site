# Gallery

This page shows some examples of cutting meshes using MCUT.

## Example 0

An armadillo is cut by a terrain-like surface.

### Input

The input meshes. 

<div class="container">
    <div style="float:left;width:49%">
	   <img src="../media/gallery/armadillo-terrain/armadillo.png" alt="xx0" />
       <p style="text-align:center;font-size:80%;">Source mesh (Armadillo)</p>
    </div>
    <div style="float:right;width:49%">
	    <img src="../media/gallery/armadillo-terrain/terrain.png" alt="xx0"/>
        <p style="text-align:center;font-size:80%;">Cut mesh (Terrain)</p>
    </div>
</div>

### Output 

The results of the cut.

<div class="container">
    <div style="float:left;width:49%">
	   <img src="../media/gallery/armadillo-terrain/armadillo_012.frag.unseale.png"  />
       <p style="text-align:center;font-size:80%;">Unsealed source mesh fragments</p>
    </div>
    <div style="float:right;width:49%">
	    <img src="../media/gallery/armadillo-terrain/armadillo_patches.png" />
        <p style="text-align:center;font-size:80%;">Cut mesh patches</p>
    </div>
</div>

<div class="container">
    <div style="float:left;width:49%">
	   <img src="../media/gallery/armadillo-terrain/armadillo_sealed.png"  />
       <p style="text-align:center;font-size:80%;">Sealed (interior) source mesh fragments</p>
    </div>
    <div style="float:right;width:49%">
	    <img src="../media/gallery/armadillo-terrain/armadillo_sealedext.png" />
        <p style="text-align:center;font-size:80%;">Sealed (exterior) source mesh fragments</p>
    </div>
</div>

## Example 1

Cutting a sphere with a torus (both watertight).

### Input

The input meshes. 

<div class="container">
    <div style="float:left;width:49%">
	   <img src="../media/gallery/sphere-torus/sphere.png"/>
       <p style="text-align:center;font-size:80%;">Source mesh (Sphere)</p>
    </div>
    <div style="float:right;width:49%">
	    <img src="../media/gallery/sphere-torus/torus.png"/>
        <p style="text-align:center;font-size:80%;">Cut mesh (Torus)</p>
    </div>
</div>

### Output

The results of the cut.

<div class="container">
    <div style="float:left;width:49%">
	   <img src="../media/gallery/sphere-torus/intersection.png"  />
       <p style="text-align:center;font-size:80%;">Intersection</p>
    </div>
    <div style="float:right;width:49%">
	    <img src="../media/gallery/sphere-torus/union.png" />
        <p style="text-align:center;font-size:80%;">Union</p>
    </div>
</div>

<div class="container">
    <div style="float:left;width:49%">
	   <img src="../media/gallery/sphere-torus/sphere-not-torus.png"  />
       <p style="text-align:center;font-size:80%;">Subtract torus </p>
    </div>
    <div style="float:right;width:49%">
	    <img src="../media/gallery/sphere-torus/torus-not-sphere.png" />
        <p style="text-align:center;font-size:80%;">Subtract sphere</p>
    </div>
</div>

<div class="container">
    <div style="float:left;width:49%">
	   <img src="../media/gallery/sphere-torus/patches.png"  />
       <p style="text-align:center;font-size:80%;">Patches </p>
    </div>
    <div style="float:right;width:49%">
	    <img src="../media/gallery/sphere-torus/sphere-unsealed.png" />
        <p style="text-align:center;font-size:80%;">Sphere fragments (unsealed)</p>
    </div>
</div>

## Example 3

<div>
  <img src="../media/mcut-zombiehead-scene.png" alt="mcut-extremely-concave-cut" style="width:70%" class="center"/> 
  <p style="text-align:center;font-size:70%;">Jagged surface cut. </p>
</div>

## Example 4

<div class="row">
  <div class="column">
    <img src="../media/mcut-cube-cut-surface-partial.png" alt="drawing1" style="width:100%"/> 
    <p style="text-align:center;font-size:80%;">Input</p>
  </div>
  <div class="column">
    <img src="../media/mcut-cube-stretched-partial-cut.png" alt="drawing3" style="width:100%"/>
    <p style="text-align:center;font-size:80%;">Result</p>
  </div>
  <div class="column">
    <img src="../media/mcut-cube-partial-cut.png" alt="drawing2" style="width:100%"/> 
    <p style="text-align:center;font-size:80%;">Stretched</p>
  </div>
</div>

## Example 5

<div>
    <img src="../media/mcut-extremely-concave-cut.png" alt="mcut-extremely-concave-cut" style="width:50%" class="center"/> 
    <p style="text-align:just;font-size:70%;">An extreme example, which is a result of cutting a source mesh that has concave polygons. The source mesh was a pentagonal frustum with the pentagons (top and bottom faces) made concave (and not parallel to each other). Each pentagon was composed of polygons with several concavities. The whole model was composed of only one volume element (all edges are on the surface). MCUT produces the correct fragments, and does not modify the connectivity except where intersected with the cut mesh. </p>
</div>

## Example 6

<div class="row">
  <div class="column">
    <img src="../media/mcut-armadillo-cut-surface.png" alt="drawing1" style="width:100%"/> 
    
  </div>
  <div class="column">
    <img src="../media/mcut-armadillo-cut-unsealed.png" alt="drawing3" style="width:85%"/>
  </div>
  <div class="column">
    <img src="../media/mcut-armadillo-cut-sealed.png" alt="drawing2" style="width:85%"/> 
    
  </div>
</div>