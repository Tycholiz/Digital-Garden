
to allow enemies to walk into a new room, you must add to the walkable area by *baking* it

misallocation of GameObjects is a major factor in poor performance.

## UI
![](/assets/images/2021-08-15-16-09-42.png)
(B) The Hierarchy window is a hierarchical text representation of every GameObject in the Scene
(C) The Game view simulates what your final rendered game will look like through your Scene Cameras
(D) The Scene view allows you to visually navigate and edit your Scene.
(E) The Inspector Window allows you to view and edit all the properties of the currently selected GameObject.
(F) The Project window displays your library of Assets that are available to use in your Project. When you import Assets into your Project, they appear here.

# Organizing files
We can use a folder structure similar to MVC architectures:

### Model
Raw data classes
- World class, Tile class, Map class etc.

### View
GameObjects

### Controller
Scripts attached to the GameObjects

# Resources
[High-quality tutorials on fundamentals of graphics in Unity (fractals, shaders, abstract shapes etc)](https://catlikecoding.com/unity/tutorials/basics/)
- this has a basis in programming and C# scripts

[Socketweaver multiplayer sdk for unity](https://www.socketweaver.com/)
- also look into Photon or Mirror