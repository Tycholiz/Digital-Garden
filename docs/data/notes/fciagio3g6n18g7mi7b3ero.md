
### Apex Engine
Since K6 is not built with node, we cannot use 3rd node modules as part of our load test script. Therefore, we are limited to the API offered by K6 and cannot use the SQS SDK provided by AWS to consume events. As a workaround, we are going to add an HTTP endpoint that will simulate the Apex Evaluation Engine. This endpoint will accept an array of simulated events (each event contains `signal`, `automation` and `capabilities`), and call a modified version of the `handleMessage` method. That is, call `evaluate`, `findActions` and `dispatchActions`.
