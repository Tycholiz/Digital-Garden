
There are 2 main threads in a RN app: the main thread  and the javascript thread
- The main thread runs on native platforms and handles displaying the elements of the UI and processes user gestures.
- The JS thread executes JS code in a separate JS engine 
	- Therefore, it deals with the business logic of the app
	- Also, it defines the structure and functionality of the UI
- These 2 threads never communicate directly and never block each other
- Between these 2 threads is the bridge, which has 3 characteristics:
	1. Asynchronous communication between the threads
	2. Batched communication between threads
	3. Serializable, meaning the threads never operate on the same data— instead exchanging serialized messages
