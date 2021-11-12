
### Mesh
A mesh is the main graphics primitive in Unity, and make up a large part of the 3D world
- meshes turn into pixels that look like real objects
- meshes can be made from either triangular or quadrangular polygons
A mesh consists of triangles arranged in 3D space to create the impression of a solid object.
- A triangle is defined by its three corner points or vertices (stored in a single array of integers; understood as each group of 3 belonging to a single vertex) <- so elements 0, 1 and 2 define the first triangle, 3, 4 and 5 define the second, and so on.

### Skybox
Skyboxes are rendered around the whole scene in order to give the impression of complex scenery at the horizon.
Internally skyboxes are rendered after all opaque objects

[How to make a Skybox](https://docs.unity3d.com/2019.2/Documentation/Manual/HOWTO-UseSkybox.html)
