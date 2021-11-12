
*Project* - whole game you are building. Contains all scenes and all assets
- you never "save your project", the only thing you save is the currently open scene. Scenes are containers which can contain one level or many levels, it's up to you to decide how you want to break things up.
*Scene* - a level in the project. A scene holds related assets. A scene may also be used for the welcome screen, a config screen, intro and other movies, and a credits.
 
Game view - the view that a player sees as they are playing the game. This is determined by the Main Camera GameObject
Scene view - the perspective of the developer building levels

### Renderer 
- A renderer is what makes an object appear on the screen. 
This class can be used to access the renderer of any object, mesh or Particle System.


### Layers
Layers allow us to define some functionality across different GameObjects

A layer has no inherent functionality, and we must give it by specifying it on the affected GameObject (e.g. a camera)
ex. we can specify an object to be ignored by the camera with the `Culling Mask` attribute on the Camera.

Layer is set at the top of the Inspector of a GameObject