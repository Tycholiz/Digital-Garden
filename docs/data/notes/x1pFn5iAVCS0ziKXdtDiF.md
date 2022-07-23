
#### Access public variable from different GameObject class
```cs
// assuming we have a PlayerController.cs script that is attached to a Player GameObject
private PlayerController playerControllerScript;

void Start() {
    playerControllerScript = GameObject.Find("Player").GetComponent<PlayerController>(); 
}

void Update() {
    // access gameOver variable from the PlayerController instance
    if (!playerControllerScript.gameOver) {
        transform.Translate(Vector3.left * Time.deltaTime * speed);
    }
}
```
