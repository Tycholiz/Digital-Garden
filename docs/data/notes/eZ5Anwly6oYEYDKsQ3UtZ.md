
#### Get user keyboard/controller input
```cs
public float verticalInput;
public float horizontalInput;

void Update()
{
    verticalInput = Input.GetAxis("Vertical");
    horizontalInput  = Input.GetAxis("Horizontal");

    // move the plane forward at a constant rate
    transform.Translate(Vector3.forward * speed);

    // tilt the plane up/down based on up/down arrow keys
    transform.Rotate(Vector3.right * rotationSpeed * Time.deltaTime * verticalInput);

    // steer the plane left/right based on arrow keys
    transform.Rotate(Vector3.up * rotationSpeed * Time.deltaTime * horizontalInput);
}

```

#### Get input based on key
```cs
if (Input.GetKeyUp(KeyCode.Space)) {
    // instantiate a GameObject
    Instantiate(projectilePrefab, transform.position, projectilePrefab.transform.rotation);
}
```

#### Run a function every 2 seconds, starting after 1 second
`InvokeRepeating("methodName", 1, 2)`

#### Get/Set(?) property of a component
```cs
private float repeatWidth;
void Start() {
    // get the x position value of the box collider component of the GameObject that this script it attached to
    repeatWidth = GetComponent<BoxCollider>().size.x / 2;
}
```