
React Native, on a conceptual level, wants to be “agnostic” to its native platform.
- this is why things like react-native-web and react-native-windows exist.

All the main activities of React Native are run on one of 3 threads:
- UI (the application’s main thread) - the Native thread where the native-level perception takes place, and it is the place where our platform (iOS/Android) carries out drawing, styling, and measuring.
- Native modules - any native modules (camera, location, photos etc.) used by the app are carried out on this thread.
- JavaScript runtime - the thread where every JavaScript application code will run.

## Bridge (current architecture)
What is known as the "React Native Bridge" is made up of 3 core components:
- Shadow Tree
- JSON (async)
- Native Modules

The bridge is basically responsible for two different behaviours: 
- defining how the UI should look and behave (via the Shadow Tree) 
    - all the UI operations are handled by a series of cross-bridge "steps" (React -> Native -> Shadow Tree -> Native UI)
- managing the native side (via Native Modules)

These communications happen via asynchronous JSON messages that get batched and sent, back and forth, over one communication channel. 
- As you may expect, this channel can get congested and lead to suboptimal experiences.

## Re-architecture
In the current architecture (at the time of writing), the Javascript and Native sides of React Native are not really aware of each other. This means that, to communicate, they rely on asynchronous JSON messages transmitted across The Bridge. These messages are sent to the native code with the expectation (but not guarantee) that they will elicit a response sometime in the future.
- a consequence of this unawareness of one another is that Native Modules need to be initialized when the app is opened, even when they are not used
    - The new TurboModules approach allows the JavaScript code to load each module only when it’s really needed, resulting in a much faster startup time for apps that use many native modules.

In the new architecture, everything will be statically typed (either with Typescript or Flow), which is seen as a source of truth. A tool called *CodeGen* will be used to automate the compatability between Javscript and Native.
- CodeGen will generate the interfaces needed by Fabric and TurboModules to send messages between JS and Native with confidence

### JSI (Javascript Interface)
The new architecture also introduces JSI, which is a layer that sits in front of the Javascript engine (currently JavascriptCore). This gives us several benefits
- Any other JS engine can be used instead (e.g. [[V8|js.v8]], Chakra)
- JavaScript can hold reference to C++ Host Objects and invoke methods on them, meaning the Javascript and Native sides will truly be aware of each other. This negates the need to serialize the messages to JSON before passing them across the bridge, removing all congestion on the bridge
    - remember, C++ has always been one of the ways to share native code across iOS and Android without Javascript, since Android’s native code is written in C\C++, and Objective-C (used by iOS) is none other than a strict superset of C

### Bridge
Instead of a single bridge to handle both the native side and the UI, the re-architecture splits these 2 concerns into 2 different components:
- Fabric, which is the re-architecture of the UI manager
- TurboModules, which is the “new gen” implementation of the interaction with native side

#### Fabric
Fabric aims to modernize the rendering layer of React Native.

Instead of all UI operations being handled by a series of cross-bridge steps (React -> Native -> Shadow Tree -> Native UI), the UI manager creates the Shadow Tree directly in C++, which reduces the number of jumps between Javscript and Native.
- the result is a greatly improved responsiveness of the UI.

Also, by using the JSI, Fabric exposes the UI operations to JavaScript as functions: the new Shadow Tree (which determines what to really show on screen) is shared between the two realms, allowing straight interaction from both ends.

