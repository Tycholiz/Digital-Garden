
# Lenses
Think of a lens as something that focuses (zooms in) on a specific part of a larger data structure
- Similar to stores  in Redux

Another way is to see lenses as little drones that copy a part of an object for us, and delve into it to change the properties, all without mutating the original 

Given a lens there are essentially three things you might want to do
- view the subpart
- modify the whole by changing the subpart
- combine this lens with another lens to look even deeper

Lenses can be handy if we have a somewhat complex data structure that we want to abstract away from calling code. Rather than exposing the structure or providing a getter, setter, and transformer for every accessible property, we can instead expose lenses. Client code can then work with our data structure using view, set, and over without being coupled to the exact shape of the structure.

It is a special `type` that combines a *getter* and a *setter* function into a single unit

The first fn is the getter, while the second is the setter
