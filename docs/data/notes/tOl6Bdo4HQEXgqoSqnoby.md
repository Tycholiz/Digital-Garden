
### Spawn
The spawn function will spawn a new process of git log type. The first argument of the function represents a path for an executable file that should start the process, and the second argument is an arguments vector that will be given to the executable. The returned process object will hold a property for each std type represented as a Stream: .stdin - WriteStream, .stout - ReadStream and finally .stderr - ReadStream.
```js
const { spawn } = require('child_process')

spawn('git', ['log'])
```

If we would like to run git log through a Node process and print it to the console we would do something like the following:
```js
spawn('git', ['log']).stdout.pipe(process.stdout)
```

Or we can do:
```js
spawn('git', ['log'], {
  stdio: 'inherit' // Will use process .stdout, .stdin, .stderr
})
```

# UE Resources
[How to launch child processes](https://www.digitalocean.com/community/tutorials/how-to-launch-child-processes-in-node-js)
https://www.freecodecamp.org/news/node-js-child-processes-everything-you-need-to-know-e69498fe970a/
