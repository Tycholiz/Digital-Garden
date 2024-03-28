
note: Native Modules are set to become deprecated, and will be replaced by Turbo Native Module and Fabric Native Components.
- see [[react-native.architecture]] for more details.

React Native Modules allow mobile apps written in React Native to access native platform APIs
- in other words, it allows JavaScript and platform-native code to communicate over the React Native "bridge"
    - this bridge handles cross-platform serialization via JSON.
- this bridge is carried out by the NativeModule system exposing instances of Java/Objective-C/C++ classes as Javascript objects. What we get in Javascript is an object that has the methods/properties that existed on the native code classes.
- ex. If you wanted to use a Couchbase Lite iOS/Android API, you would implement a React Native plugin that exports the Couchbase Lite Android and iOS APIs to Javascript.
- ex. If you wanted to use the phone's native APIs (e.g. Calendar API), we would first write native code that communicates with those native APIs, then we would make that native code invokable through Javascript via the bridge.
