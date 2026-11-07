
## Local vs Global
(setting found at the top, next to the QWERTY tools)
This refers to `local` or `global` coordinates. If we are set to use `local`, then the directional arrows (x,y,z) of the GameObject will change with the moving object. If we are set to use `global`, then they will stay the same, even if the GameObject moves/rotates.
- Imagine we have a ball rolling down a slope. If set to `local`, then the y-axis will move with the ball, since that is consistent with the coordinate system of the GameObject itself (ie. the local coordinates). If however we set it to `global`, then the y-axis would always point upward.

* * *

#### Importing `.unitypackage` files
From the top menu in Unity, 
select `Assets > Import Package > Custom Package`, then find the `.unitypackage` file in the file explorer.
