
There are 3 components to implementing a [[contract test|testing.method.contract]] with Pact:
- Consumer - initiates the HTTP request.
- Pact Mock Provider
- Interaction

### Artifacts
#### Contract
Pact is another word for a contract.

A Pact defines:
- the consumer name
- the provider name
- a collection of interactions
- the pact specification version

##### Pact verification
The contract must be verified, so the requests contained in a Pact file are replayed against the provider code, and the responses returned are checked to ensure they match those expected in the Pact file.

* * *

## Broker process
Once we have the interactions generated from the consumer-side unit Pact tests, we move to the provider side. Pact takes that list of interactions and replays them onto your server (ie. Pact makes HTTP requests to your live server)
- Running the consumer side of the pact contract tests generates the contract itself, which is then used by Pact to send the same requests to your actual provider. 
    - This contract (pact) will be stored in the pact broker.
- The provider then pulls the contract from the pact broker and replays this request against their local environment, by verifying the request and response match with the consumer requirements.

There are a number of choices that can be made here, but usually these are the choices:
- Invoke just the controller layer (in an MVC app, or the "Adapter" in our diagram) and stub out layers beneath.
- Execute the code up to he service/business logic layer, but mock out the data persistence layer
- Choosing a real vs mocked out database
- Choosing to hit mock HTTP servers or mocks for external services

Generally speaking, it's easiest to test the entire service and mock out external services such as downstream APIs (which would need their own set of Pact tests)
- this is what gives us some of the benefit offered by [[integration tests|testing.method.integration]] without the high cost of maintenance.

If the database can be easily set up/torn down locally, then the real database is probably easiest to use. If necessary, you can just stub out the data persistence layer.

* * *

Pact will ensure that the provider returned the expected object, but we must test that our code receives the right object.
- this is often the same as the object that was in the network response.

The Pact Framework is of course called from within the client's testing framework (e.g. [[jest|jest]]).

The Pact Framework offers a Pact Mock Provider and a Pact Mock Consumer.

## How Consumer Testing works
1. Before any tests are run, we write the interaction, specifying the expected request and response.
2. The consumer test code then fires a real request to a mock provider (created by the Pact framework).
3. The mock provider checks to see if the request matches the expected request from the interaction.
4. Assuming it was successful, the consumer test code confirms that the response was correctly understood.

## Interactions
Each pact is a collection of interactions. An interaction is a request-response pair.
- The first step in writing a pact test is to describe this interaction.

Each interaction is considered to be independent. This means that each test only tests one interaction

Pact tests operate on each interaction. They ask the question, "assuming the provider returns the expected response for this request, does the consumer code correctly generate the request and handle the expected response?"

Usually, the interaction definition and consumer test are written together
- the interaction says "upon receiving a POST request to /event, with an `event` object in the body, we will respond with 200 OK"
- the test triggers the client code to generate the request and receive the response

If we pair the consumer test and provider verification process for each interaction, the contract between the consumer and provider is fully tested without having to spin up the services together.

Interactions for HTTP and Messages look different:
- For HTTP:
    - An expected request - describing what the consumer is expected to send to the provider
    - A minimal expected response - describing the parts of the response the consumer wants the provider to return.
- For messages:
    - The minimal expected message - describing the parts of the message that the consumer wants to use.
### The pact describes a series of interactions
Once all of the interactions have been tested on the consumer side, the Pact framework generates a pact file, which describes each interaction:

![](/assets/images/2022-01-19-15-36-29.png)

### Provider state
If you need to describe interactions that depend on pre-existing state, you can use provider states to do it. Provider states allow you describe the preconditions on the provider required to generate the expected response - for example, the existence of specific user data. 
- ex. Instead of writing a test that says “create user 123, then log in”, you would write two separate interactions - one that says “create user 123”, and one with provider state “user 123 exists” that says “log in as user 123”.

The provider should be in a particular state when a given request is replayed against it
- e.g. “when user John Doe exists” or “when user John Doe has a bank account”. 
- These allow the same endpoint to be tested under different scenarios.

A provider state name is specified when writing the consumer specs, then, when the pact verification is set up in the provider the same name will be used to identify the set up code block that should be run before the request is executed.

## How Provider Verification works
With provider verification, each request is sent to the provider, and the actual response it generates is compared with the minimal expected response described in the consumer test.
- Provider verification passes if each request generates a response that contains at least the data described in the minimal expected response.

In many cases, your provider will need to be in a particular state (such as "user 123 is logged in", or "customer 456 has an invoice #678")
- Pact enables us to do this: it allows us to set the provider state before the interaction is replayed.

Unlike Consumer testing, provider verification is entirely driven by the Pact framework

* * *

### Random Data
Avoid random data. If you are using a Pact Broker to exchange pacts, then avoid using random data in your pacts. If a new pact is published that is exactly the same as a previous version that has already been verified, the existing verification results will be applied to the new pact publication. This means that you don't have to wait for the provider verification to run before deploying your consumer - you can go straight to prod. Random data makes it look like the contract has changed, and therefore you lose this optimisation.


# Resources
- [Example](https://github.com/YOU54F/jest-pact-typescript/blob/master/src/pact/client/requestPathMatching.pacttest.ts)
- [Another Example](https://github.com/alessioerosferri/pact-demo)
# UE Resources
- [non-HTTP testing](https://docs.pact.io/getting_started/how_pact_works/#non-http-testing-message-pact)