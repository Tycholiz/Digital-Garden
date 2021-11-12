
Naturally, components can be controlled from scripts.

#### Apply force to a GameObject
For instance, cause a player to jump, cause an enemy to fly backwards when hit etc.
```cs
private RigidBody playerRigidBody
void Update() {
    if (Input.GetKeyDown(KeyCode.Space)) {
        playerRb = GetComponent<Rigidbody>();
        playerRb.AddForce(Vector3.up * 1000);
    }
}
```

#### Detect a collision with the GameObject
Define a new method on your script's main class
```cs
private void OnCollisionEnter(Collision collision) {
    // do the thing when there is a collision
    if (collision.gameObject.CompareTag("Ground")) {
        isOnGround = true;
    } else if (collision.gameObject.CompareTag("Obstacle")) {
        Debug.Log("Game over, bro");
        gameOver = true;
    }
}
```