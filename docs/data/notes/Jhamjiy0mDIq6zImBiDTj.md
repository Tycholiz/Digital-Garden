
There are 3 components to implementing a [[contract test|testing.method.contract]] with Pact:
- Consumer - initiates the HTTP request.
- Pact Mock Provider
- Interaction

With Pact testing, a service is never talking directly to another service. Instead, we only ever test one application at a time, and we're only going to capture its view of the integration point.
- the consumer will talk to a mock of the provider (supplied by Pact)
- Pact pulls all of the contracts (from all consumers) from the Pact Broker and replays the requests against the provider (contract validation)

Pact tests will only fail if the specific subset of the data that the consumer cares about is changed on the producer side. If we are making breaking changes to the provider, then we'll know it as soon as we run the provider contract tests (can even be run locally).
- ex. if an API endpoint offers 3 fields (`make`, `model`, `year`) but our consumer only uses the first 2, if the provider changes its API to remove the `year` field, the contract test will not fail.

## How it works
### Consumer-side
1. For each integration point with a given provider, we write the interaction in the consumer (commonly as a unit test), specifying the expected request and response.
    - therefore, each route will have a unit test

Running the unit tests in CI/CD...

2. The consumer fires a real request to a mock provider (provided via Pact framework).
3. The mock provider checks to see if the request matches the expected request from the interaction.
4. Assuming it was successful, the consumer confirms that the response was correctly understood.
5. A file is generated that records all of the interactions between the consumer and the provider. This record is called a pact, and it is published to the Pact Broker (*pact publication*).
    - a new *consumer version* will be generated any time the consumer changes
      - Multiple consumer versions can all refer to the same pact version (this happens when changes to the consumer don't change the pact).
      - A specific version of the consumer (ie. consumer version) will always only have one pact version for each provider.
    - a new *pact version* will be generated only when the contents of the pact change (ie. the request/response shape)

![](/assets/images/2022-11-07-12-40-45.png)

6. (can-i-deploy; optional) this part of the Pact system answers the question "is it safe to deploy this consumer to the specified environment?"
    - the first time, the answer will be NO, because the Provider has never verified the contract. In other words, there is no Provider in production that satisfies this contract.

### Provider-side
Once the consumer-side is done (ie. once we've created pacts for the consumer), we need to validate the pacts against the provider.

1. We run our provider server locally (e.g. if our provider is an express server, then we start the server)

Running the unit tests in CI/CD...

2. We write a unit test that fetches all the pacts for the provider and checks that the provider can satisfy them by running them against the URL for the provider (which we supply in the provider unit tests).
    - `return new Verifier(opts).verifyProvider()`, which starts the mock server.
3. The result of the testing will then be sent back to the Pact Broker to record which *provider version* can/cannot satisfy which *pact verions*.
4. (can-i-deploy; optional) answers the question "is it safe to release this *provider version* to production?"
5. Deploy: since the first time around, the consumer is not in production (step 6 above), so it will be safe to move this provider version to production.
6. Check the deployed pact broker, and the contract should be green (verified)
7. Re-run the consumer pipeline. This time the can-i-deploy step will pass and the consumer will be deployed to the specified environment.

For each interaction in a pact file, the order of execution is as follows:
- `BeforeEach` -> `StateHandler` -> `RequestFilter (pre)` -> `Execute Provider Test` -> `RequestFilter (post)` -> `AfterEach`

### Broker process
Once we have the interactions generated from the consumer-side unit Pact tests, we move to the provider side. Pact takes that list of interactions and replays them onto your server (ie. Pact makes HTTP requests to your live server)
- Running the consumer side of the pact contract tests generates the contract itself, which is then used by the Pact broker to send the same requests to your actual provider. 
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

## Provider Verification
Verification ultimately tells us if a particular pact version will work with a particular version of the provider.
- when we run the verification, Pact fetches the contracts and replays each request against the provider and ensures that the provider's response matches the consumer's expectations as recorded in the pact. If the two match, then we know the consumer and provider are compatible.

To verify a pact we must:
1. configure the URL of the pact (a path of the pact broker URL)
2. set up data for the provider states
    - provider states allow us to set up data on the provider by injecting it straight into the data source right before the interaction is run. In this way, it can make a response that matches what the consumer expects.
3. configure and run the provider app so that we can run the requests against it.

Every time we run a verification in CI/CD, the results are published to the Pact Broker (known as *verification result*).

Whenever the provider changes, it is considered as a new *provider version* (identified at a minimum by git commit SHA + git branch)
- as with a consumer, you should associate a particular version of the provider with 1+ branch.

With provider verification, each request is sent to the provider, and the actual response it generates is compared with the minimal expected response described in the consumer test.
- Provider verification passes if each request generates a response that contains at least the data described in the minimal expected response.

In many cases, your provider will need to be in a particular state (such as "user 123 is logged in", or "customer 456 has an invoice #678")
- Pact enables us to do this: it allows us to set the provider state before the interaction is replayed.

Unlike Consumer testing, provider verification is entirely driven by the Pact framework

* * *

### Random Data
Avoid random data. If you are using a Pact Broker to exchange pacts, then avoid using random data in your pacts. If a new pact is published that is exactly the same as a previous version that has already been verified, the existing verification results will be applied to the new pact publication. This means that you don't have to wait for the provider verification to run before deploying your consumer - you can go straight to prod. Random data makes it look like the contract has changed, and therefore you lose this optimisation.

* * *

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

# Resources
- [Example](https://github.com/YOU54F/jest-pact-typescript/blob/master/src/pact/client/requestPathMatching.pacttest.ts)
- [Another Example](https://github.com/alessioerosferri/pact-demo)
# UE Resources
- [non-HTTP testing](https://docs.pact.io/getting_started/how_pact_works/#non-http-testing-message-pact)