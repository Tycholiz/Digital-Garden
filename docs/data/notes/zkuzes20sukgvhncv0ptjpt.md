
A facade is a wrapper that provides a simpler interface to a more complex one.
- it is about encapsulating a complex subsystem within a single interface object. 
- an added benefit is it also promotes decoupling the subsystem from its potentially many clients
- ex. AWS offers [[lib packages|aws.SDK#lib-packages,1:#*]] alongside their lower level packages, as a way to simplify the commands and make it easier to use.
- using a facade notably does not prevent users from bypassing the facade and engaging directly with the more complex layers, but gives them the option to interact with a simplified interface.

A facade differs from an [[adapter|general.patterns.structural.adapter]], since a facade creates a new interface, while an adapter re-uses the old interface.

![](/assets/images/2022-04-11-12-33-11.png)