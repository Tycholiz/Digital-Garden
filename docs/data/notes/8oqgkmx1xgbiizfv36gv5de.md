
The need to maintain backward and forward compatibility appears in many [[distributed|deploy.distributed]] systems

- *Backward compatibility* is making new code compatible with existing data. 
- *Forward compatibility* is making existing code compatible with new data.

Backward compatibility is straightforward. It’s the ability to open old documents in new versions of the program.

Forward compatibility, the ability to open documents in formats invented in the future, is rarer. We can see forward compatibility in web browsers, which are written so the features added to HTML won’t break the ability to render new sites in old browsers.

The publisher of an API must balance their desire to make changes against their customers' reluctance to change something that works for them.
- The result is strong pressure to preserve backward compatibility over time, often across many versions.

An API publisher can intuit that an endpoint can respond with additional data, trusting existing clients to ignore it, but not require additional data in requests, because existing clients won’t know to send it. 

Stripe [solved this problem](https://stripe.com/blog/api-versioning) by building a middleware system through which all requests and responses to its API travel. It translates between the latest version of the Stripe system and the API version that the client is using. As a result, Stripe developers don't have to worry as much about nuances with old API versions, since the middleware ensures the old version will be converted to comply with the latest version. When Stripe developers want to create a new API version, they just need to add a new stack to the middleware.
![](/assets/images/2022-03-18-13-01-30.png)

## Tools
- [Cambria](https://github.com/inkandswitch/cambria-project)
  - helps us achieve campatability between schema versions— You specify (in YAML or JSON) a lens, which specifies a data transformation.
  - takes lessons from Stripe's middleware approach to compatability (described above)
  - [Intro and overview of Cambria](https://www.inkandswitch.com/cambria/)

## E Resources
https://www.inkandswitch.com/cambria/