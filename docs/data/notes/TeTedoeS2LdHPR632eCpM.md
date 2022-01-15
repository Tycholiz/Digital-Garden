
# Google Cloud
## Terminology
### Reference 
- a locally existing pointer that points to a location within the cloud where data is stored. The client (ex. mobile app) can interact with the reference in order to interact (CRUD) with the database.

### Task 
- an object that is returned from calling a CRUD operation on a `reference` (ex. `.putFile()`. Holds info similar to `res` object in Express.js

### Cloud functions
- A service from Google that allows you to run snippets of code within their own infrastructure. If you were to build your own server, you would simply execute these functions in your backend code. Normally they are executed in response to some event. Since Firebase is a Backend as a Service, we don't have this ability. Cloud functions allow us to fulfill this need.
- The functions can also be fired directly with a simple HTTP request.
- Normally however, we have an event provider (the origin of the event) such as Firestore that will wait on a certain event to occur 
	- ex. we can execute functions in response to databse writes, user creation etc.
- can be used as a webhook - Via a simple HTTP trigger, respond to events originating from 3rd party systems like GitHub, Slack, Stripe, or from anywhere that can send HTTP requests.

#### Features
- these functions are stateless, meaning that if we were trying to make a counter function that holds the current value, we would get unpredictable results. This is because multiple instances of the same function will be created depending on how many people are using your app and executing that function. Therefore, all storage needs must be delegated to some other Google cloud service (like Firestore) 

# Firebase
- When you create a new Firebase project in the Firebase console, you're actually creating a Google Cloud Platform (GCP) project behind the scenes
	- You can think of a GCP project as a virtual container for data, code, configuration, and services. A Firebase project is a GCP project that has additional Firebase-specific configurations and services (put another way, a firebase project is a wrapper around a GCP project). 
	- therefore, a Firebase project ***is*** a GCP project. Everything that is possible in GCP is also possible in Firebase.
