
Could have been called a "microservice test", since it involves testing the microservice as a whole.

Unit testing is a way to test behaviour within a specific module, integration testing is a way to test multiple modules as a group, and Component testing is a way to verify that the (micro)service works as a whole to satisfy the business requirements. 
- therefore it is about testing all of the modules that work together toward some business requirement.
- naturally, there may be some modules of a service that don't get tested during a component test, if they do not directly contribute to the service's business requirement.

you can think of component testing as end-to-end testing an entire service that is isolated from external dependencies and any collaborating microservices with [[test doubles|testing.test-double]].
- By writing tests at this granularity, the contract of the API is driven through tests from the perspective of a consumer. Isolation of the service is achieved by replacing external collaborators with test doubles and by using internal API endpoints to probe or configure the service. 
