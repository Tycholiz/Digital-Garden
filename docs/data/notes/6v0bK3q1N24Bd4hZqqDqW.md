
#### Add sound effects to a Player/Unit
1. add a variable of type `AudioClip` in a script attached to the GameObject to emit sounds (like `PlayerController`)
2. attach the asset to the script (ie. pass it in as parameter in the Inspector)
3. add an `Audio Source` component to the Player/Unit
4. in the script file, get ahold of the component and run `audio.PlayOneShot` during the event.
```cs
// ...
    private AudioSource playerAudio;
    public AudioClip jumpSound;
    public AudioClip crashSound;

    void Start() {
        playerAudio = GetComponent<AudioSource>();
    }

    void Update() {
        // Jump
        if (Input.GetKeyDown(KeyCode.Space)) {
            // ...
            playerAudio.PlayOneShot(jumpSound, 1.0f);
        }
    }
```
