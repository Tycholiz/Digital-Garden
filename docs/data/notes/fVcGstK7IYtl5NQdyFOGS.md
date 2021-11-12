
Prefabs are equivalent to the concept of components in React.

Prefabs are a special type of component that allows fully configured GameObjects to be saved in the Project for reuse among scenes.
- A major benefit to Prefabs is that they are essentially linked copies of the asset that exist in the Project window, so changes made to the original prefab will propagate to the rest.
    - furthermore, you can edit a single instance of a prefab, but it will not necessarily propagate to others 
        - the `revert` button allows us to go back to the master prefab
        - the `apply` button allows us to apply the changes of the current prefab to the master, and therefore to all instances of it as well.

Prefabs are created automatically when an object is dragged from the Hierarchy into the Project window. 
- prefabs look similar to other objects that appear in the Project window. When selected, you'll see that they are of filetype `.prefab`
- When prefabs are present in the Hierarchy, they’re represented with blue text and a blue cube.

When you convert an object to a Prefab, a new set of buttons is added above the Transforms values:
![](/assets/images/2021-08-19-21-21-55.png)

#### Creating from existing GameObject
Drag GameObject from Hierarchy window into `prefab/` directory and choose to create `Original Prefab`

#### Making changes to an instance and propagate changes to Prefab
After making the changes, select a GameObject and in the top right of the Inspector window select `Overrides` and `Apply All`