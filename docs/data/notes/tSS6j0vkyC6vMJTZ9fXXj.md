
#### Get component of a GameObj within a script
```cs
private Rigidbody playerRb;
// ...
// can also pass any component, like `Collider`
void Start() {
    playerRb = GetComponent<Rigidbody>();
}
void Update() {
    // cause player to jump (obvs we need a Spacebar click-handler here)
    playerRb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
}
```

