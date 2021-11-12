
#### Attach particle to another GameObject's action (e.g. Projectile GameObject)
1. set the particle GameObject to be a child GameObject of the Projectile
2. get the particle from within the Projectile's script: 
    - `public ParticleSystem explosionParticle;`
3. call `Play()` on the object: `explosionParticle.Play();` from within the condition that would trigger it. (e.g. on collision of projectile)
4. attach the particle GameObject to the Projectile for the `Explosion Particle` param.
```cs
// ...
public ParticleSystem explosionParticle;
// ...
private void OnCollisionEnter(Collision collision) {
    if (collision.gameObject.CompareTag("Ground")) {
        explosionParticle.Play();
    }
}
```
