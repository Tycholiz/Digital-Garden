
The animator is a [[state machine|general.patterns.state-machine]]

### Animation Layers
Layers serve to help us organize the complex state machine that is our Animator Component
We can use *Layers* to manage the state machine for a specific body part, or specific action
- ex. Head, Body
- ex. walking, jumping, running, dying, crouching

We can also use Layers in such a way where we have a layer for lower-body, and one for upper-body

On each layer, you can specify the mask (the part of the animated model on which the animation would be applied), and the Blending type. Override means information from other layers will be ignored, while Additive means that the animation will be added on top of previous layers.

#### Mask
The Mask property is there to specify the mask used on this layer. For example if you wanted to play a throwing animation on just the upper body of your model, while having your character also able to walk, run or stand still at the same time, you would use a mask on the layer which plays the throwing animation where the upper body sections are defined, like this:
![](/assets/images/2021-08-26-11-07-48.png)
Note: the `M` symbol denotes that the layer has a mask applied
