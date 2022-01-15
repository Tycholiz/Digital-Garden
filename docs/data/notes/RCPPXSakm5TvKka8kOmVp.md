
### Anchors (`&`) and Aliases (`*`)
can be used to duplicate content across your yml file.
- with it, we can duplicate or inherit properties

Anchors are identified by an `&` character, and aliased by an `*` character.

You can use YAML anchors to merge YAML arrays.

Below `&flag` identifies the Apple item, and is later referenced.
```yml
- &flag Apple
- *flag
```

Becomes this when the yml is parsed:
```yml
- Apple
- Apple
```

### Overrides (`<<`) a.k.a. map merging
This is a special key that indicates key-values from another mapping should be merged into this mapping.

This example demonstrates 2 features:
- Anchors can be used with non-scalar values, such as whole objects (here the `nodeinfo` object is referred to in the `echoit` container)
- Overrides, which allows us to merge the key-value pairs of `&function` into the `echoit` object
    - note: that the `image` key-value gets overwritten, since we define it even though it already exists in `&function`
```yml
# docker-compose.yml
services:
  nodeinfo: &function
    image: functions/nodeinfo:latest
    labels:
      function: "true"

echoit:
    <<: *function
    image: functions/alpine:health
```
