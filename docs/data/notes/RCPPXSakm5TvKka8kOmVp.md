
YAML stands for *Yaml Ain't a Markup Language*.

YAML is excellent for configuration files and alike thanks to anchors and aliases that prevent you from having sparse chunks of text, as well as comments which allow you to clearly explain the logic behind a given setup, etc.,

Rather than parse and stringify, YAML uses the following terms:
- *load* - a load is the process of translating a character stream to data structures (the actual data you operate with).
  - Parsing is a step of that process, yet the YAML processor has additional tasks to perform than to simply parse a source.
- *dump* - the reverse operation of serializing data back to text.

YAML doesn't support extending arrays, so when we override an array value, we need to provide all the contents of the array, not just the diff

## Directives
A directive is an instruction you can supply to the YAML processor. One can pick from two directives:
- `YAML`
- `TAG`

`---` is not a "document start" marker. This particular notation stands for "directives end".
- YAML allows you to specify multiple documents in a single stream (e.g. file), by separating them with `—--`

## Code reuse
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

* * *

### Strings in YAML
When putting a string over multiple lines
- `|` will preserve all new lines
- `>` will fold them into a single line

```yml
|
Winnie-the-Pooh
Винни-ПухCOPY
```

The equivalent string in a JSON document would be:
```json
"Winnie-the-Pooh\nВинни-Пух\n"COPY
```

Now, if the folded style was used, the output would be considerably different:
```yml
>
Winnie-the-Pooh
Винни-ПухCOPY
```
Would result in:
```json
"Winnie-the-Pooh Винни-Пух\n"
```