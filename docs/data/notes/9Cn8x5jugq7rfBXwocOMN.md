
### Follow player
In a C# script (attached to the Main Camera), we can specify a new public variable of type `GameObject`. Then we can modify that variable's position attribute in the `update()` method. Then, as a final step we can drag an actual GameObject that we have defined (such as our Player) onto the script, as a way to pass the GameObject to the script.
- this would be getting a reference to the Player GameObject
```cs
    public GameObject player;
    // Update is called once per frame
    void Update()
    {
        transform.position = player.transform.position;
    }
```
