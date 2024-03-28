
### Audio source
#### spatial blend 
This property allows us to increase the volume of objects as a character gets nearer to them

#### rolloff 
this property defines its range in 3D space, and the rate at which it fades at greater distances and becomes inaudible. You can set the rolloff of your sounds to simulate the ways that different sounds carry.

#### 3D sound settings 
this property is the quality of audio that it changes orientation based on where the player is.
- accomplished with an Audio Listener component (attached to MainCamera)

- the the parameters of the 3D Sound Settings property control how volume and pitch can change based on the positions of the Audio Source and the Audio Listener.  
![](/assets/images/2021-08-16-17-07-11.png)

#### min/max distance
If a character is closer to a source of sound than the `minimum distance` (ie. character distance < minimum distance), then the audio plays at max volume
The `max distance` will define the most quiet that this object can possibly be
- min and max denoted by the thin blue spheres around the red pot in the centre
![](/assets/images/2021-08-16-17-14-38.png)

