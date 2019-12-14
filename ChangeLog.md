#### 11/12/2019
  - point imposter (using point sprites, geometry shader) ready.
  - line imposter (using point sprites, geometry shader) ready.

#### 10/12/2019 
  - simplified the APIs of drawables (i.e., shader management is now taken care of by the drawables, so client code only needs to call the draw() function for rendering);

#### 02/10/2019 
  - added various KD-Tree implementations and API;
  - added point cloud normal estimation and re-orientation;
  - added plane extraction from point clouds using RANSAC;
  - add Poisson surface reconstruction.

#### 19/04/2019
  - Easy3D 2.0 released. Major changes are
    - reorganized the classes/functions in modules (i.e., core, viewer, fileio, util, algo);
    - added a few rendering techniques (e.g., shadow, eye-dome lighting, ambient occlusion, transparency);
    - added a few tutorial examples.

#### 11/12/2018
  - The first release of Easy3D;
  - Tested on macOS, Linux (Ubuntu), and Windows.