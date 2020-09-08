# Gallery

This page shows some examples of cutting meshes using MCUT.

## Armadillo cut

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

## Sphere torus CSG

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