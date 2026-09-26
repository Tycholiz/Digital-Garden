
When we run `react-native start` both a node.js server and metro bundler are started.
- Metro can be thought of as similar to "webpack for react native"

Metro is a JavaScript [[bundler|js.bundler]]; It takes in an entry file and various options, and gives you back a single JavaScript file that includes all your code and its dependencies.
1. Metro builds a dependency graph of all modules required from the entry point (ie, our whole react-native app)
2. Metro transforms modules into a format that React-Native can understand
3. Metro serializes the transformed modules to form a bundle
	- A bundle is just a bunch of modules combined into a single js file.
4. The bundlefile gets installed on the device, where its code is then executed.
	- Remember that when you are writing code for a React Native application, your code is not "translated" to Java/Swift/whatever. The Native Modules will send events to a Javascript thread, and the JS thread will execute your bundled React Native code.

I/O of Metro
- Input - entryfile along with any options
- Output - the bundle

Metro bundler is not used for production
- ex. running `react-native run-ios --configuration Release`

### The Bundle
The bundlefile can be found at `localhost:8081/index.bundle?platform=ios&dev=true&minify=false`. 
- This is the API through which the device will access the bundle
- this is served from memory, therefore does get written into our project directory
- The bundle may be stored in different formats, such as binary or .bundle file.
- Metro bundler also translates JSX to standard javascript
