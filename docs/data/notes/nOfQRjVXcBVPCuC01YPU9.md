
#### search for pattern in a specific filename
- ex. search for pattern 'react' within all package.json
```sh
find . -name 'package.json' | ack -x 'react'
```

This has a flaw though, where it will only ack the first result from find.
