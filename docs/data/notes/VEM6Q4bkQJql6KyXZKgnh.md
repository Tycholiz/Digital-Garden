
### RigidBody
`RigidBody` gives a GameObject physical properties so that it can interact with gravity, air density, and other GameObjects. 
RigidBody can be used to determine if an object should be frozen in 2D/3D space or not

### Materials
`Materials` are components that define the surface characteristics of objects and how those surfaces interact with light. In each new 3D Scene, directional light is included to simulate the sun.

You can drag a PNG into the assets folder of a Unity project, and by doing so, it will become a material (subfolder of `assets/`) that can be dragged onto an object of the scene

#### Physic Materials
With Physic Materials, you can make an object bounce and change its friction and drag properties.These properties take effect when the object is under the effects of gravity.
- in project window, right-click for dropdown and `create > physic material`
- Collider components

### Colliders
The sense of collision, in that you cannot pass through it.
A collider has a material property, giving us the ability to drag one of our materials on top to give characteristics to the collision of an object
- ex. if we have a physic material that gives the quality of bounciness, then we can apply that to the collider, which makes the underlying GameObject bouncy. 

Objects like spheres and cubes by default have collision

Colliders require a RigidBody to detect collision in our physics

