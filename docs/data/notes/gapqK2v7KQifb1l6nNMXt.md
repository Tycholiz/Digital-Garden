
#### Get reference to GameObjects that have a Tag
```cs
enemy = GameObject.FindWithTag("Enemy");
// or find multiple
enemy = GameObject.FindGameObjectsWithTag("Enemy");
```

#### Check to see if the current gameObject has a particular tag
```cs
if (transform.position.x < tippingPoint && gameObject.CompareTag("Obstacle")) {
    Destroy(gameObject);
}
```