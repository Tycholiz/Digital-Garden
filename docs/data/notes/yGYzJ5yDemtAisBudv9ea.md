
WASM is the answer to the question "how do we run languages other than Javascript in the browser?"
- any language can run as a WASM language

Conceptually, what is happening is that there is a container within the browser that includes whatever runtime and language environment that we need, and it interfaces with a minimal amount of javascript that is running natively in the browser.

WASM becomes useful when there is an intense amount of computation happening in the browser
- ex. Figma
