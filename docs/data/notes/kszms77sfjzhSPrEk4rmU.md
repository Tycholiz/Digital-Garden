
A shader's role is to convert 3D assets into 2D pixels onto the screen.


Consider that a triangle (in 3D space) is made up of three 3D points (vertices), for example:
```
(-1.03, 000,   0.00)
(0.69,  0.00,  1.00)
(0.69,  0.00, -1.00)
```
To show a triangle on the screen, we need to convert these 3D points into 2D points onto a virtual screen (called a Viewport)
- To figure this out, we need to consider the position/rotation of the triangle, as well as the position/rotation of the camera itself.
- All transformations required to convert a 3D point into a 2D point in the viewport can be encoded in a single 4x4 matrix:
```
[0, 0, 0, 0]
[0, 0, 0, 0]
[0, 0, 0, 0]
[0, 0, 0, 0]
```

When you write a Shader, you are primarily responsible for writing a *vertex function* and a *fragment function*. The graphics system makes use of these functions, and handles the rest.
- vertex function is responsible for conversion from 3D to 2D (most of the complicated math can be handled for us by utilizing APIs)
- fragment function is responsible for determining which color each pixel within the triangle will be (ie. The fragment function returns a color)
    - this occurs right after rasterization.

Vertex function is also responsible for telling the interpolation system what information to interpolate.
- whatever data is returned from the vertex function will be interpolated for each pixel and sent to the fragment function:

Vertex function runs once for each vertex in the triangle (ie. 3 times per triangle)
Fragment function runs once per pixel that is covered by the triangle.
The rasterization and interpolation step ties the vertex function and fragment function steps

Vertex Function -> position, UV etc -> Interpolation system -> Fragment function -> pixel color to be written to the screen

### Rasterization
With a 2D triangle on the screen, rasterization is the process of figuring out which pixels in the screen belong within the triangle.

## Lighting
