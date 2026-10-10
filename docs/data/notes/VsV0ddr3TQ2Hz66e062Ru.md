
### Sorting
spec: SortingLayer should be used in favor of z-index.

the render order of Renderers through their `Render Queue`.

There are 2 types of Render Queue:
1. Opaque queue
2. Transparent queue (the main one); this contains the `Sprite Renderer` , `Tilemap Renderer`, and `Sprite Shape Renderer` types.

The Camera component sorts Renderers based on its Projection setting, of which there are 2 options:
1. Perspective - the sorting distance of a Renderer = the direct distance of the Renderer from the Camera's position.
2. Orthographic - The sorting distance of a Renderer = the distance between the position of the Renderer and the Camera along the Camera’s view direction. 

sprites that belong together can be put into a `Sorting Group`, which share the same `Sorting Layer`, `Order in Layer` and `Distance to Camera`
