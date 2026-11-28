
Development for Unity is for a large part procedural in nature, not object-oriented. Unity is component-based. You're working with collections of data with behavior attached. This sounds like objects, but encapsulation and identity protection are not at all part of the architecture. Trying to force Unity to always adhere to a strict OOP standard is not practical. In fact, Unity Technologies is shifting focus to data-driven-design with ECS (entity component system) and DOTS (data oriented technology stack).
- Data-oriented design puts the emphasis of coding on solving problems by prioritizing and organizing your data to make access to it, in memory, as efficient as possible. This contradicts the object-oriented principle that the design of code should be led by the model of the world you are creating.

The typical object-oriented workflow is to:
1. Create a GameObject
2. Add components to it
3. Write `MonoBehaviour` scripts that change the properties of these components

## Update
`update()` runs on every frame. One device might get 20 fps, and another might get 60 fps, so the function will be called a disproportionate and unpredictable amount of times. Therefore we need to factor a time-frame into our calculations for code inside `update()`
- For instance, if we want to move a vehicle forward (the higher the device's fps, the lower the `Time.deltaTime` value will be)
    - If you multiply a value by `Time.deltaTime`, it will change it from 1x/frame to 1x/second

```cs
// move ~20 meters/second as long as the user is pressing up arrow
transform.Translate(Vector3.forward * Time.deltaTime * 20 * horizontalInput)
```
Vector3.forward is shorthand for `(0,0,1)`, so when we multiply a vector, each value of the vector is multiplied. Therefore, a vector multiplied by an integer results in a vector
- ie. `Vector3.forward * 20` results in `(0,0,20)`

## Working with GameObjects
You can find a GameObject in the scene like this:
```cs
private GameObject playerObj = null;

private void start() {
    if (playerObj == null) {
        playerObj = GameObject.Find("Player");
    }
}

```

Then you can get access to things like position:
```cs
if (playerObj.transform.position.x > leftBoundary);
```

`MonoBehavior` is a class that is meant to be added to a component on a GameObject
- Objects descended from `MonoBehavior` get instantiated through background serialization taking place in Unity

## Misc
Script template path:
`/Applications/Unity/Hub/Editor/2020.3.16f1/Unity.app/Contents/Resources/ScriptTemplates`

[gitignore template](https://github.com/github/gitignore/blob/master/Unity.gitignore)
[.gitattributes file (for git-lfs)](https://gist.github.com/Tycholiz/d9d11c2e8cc8c0c898addc80631c5294)
