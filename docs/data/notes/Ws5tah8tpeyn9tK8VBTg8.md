
# How React Native Works
- When we `debug remotely`, we are running our JS code in the browser, as opposed to on the device.
	- when we do this, a web socket connection is made between the device and the browser
	- Since this is a different JS engine than would be used in production, it's possible that errors occur in one but not the other.
- You can consider React Native as a browser that instead of rendering divs, spans, h1, inputs it renders instead native components. The nice thing is that the native components run in their own native thread (that means you get native performance) and your javascript runs in it's own thread and orchestrates all the native components.
- In production, when the application starts, it starts running code from your javascript bundle and that will drive which components are to be created on the screen.
- In development, React Native uses watchman to detect when you've made code changes and then automatically build and push the update your device without you needing to manually refresh it.

## Building the app (run-ios/run-android)
- When we run `react-native run-ios`, a list of iOS simulators is searched, and the default one is started.
- Since by default it is run in Debug mode (ie. development mode), a series of `xcrun` commands are run, culminating in the `xcodebuild` commands being run, which results in the app being built on the device.
- Once the app is successfully built, a request is sent to the metro bundler URL (API at port 8081 mentioned above). Metro will bundle the javascript in response to the app having been built.
	- In Release mode (ie. production), the bundle is pre-packaged, which is why we need to change where the ios device looks for the bundle (`App.Delegate.m` file)
