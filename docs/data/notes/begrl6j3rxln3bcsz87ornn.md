
The purpose of AWS X-Ray is to analyze and debug distributed applications. It helps us understand how the application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors.

X-Ray provides an end-to-end view of requests as they travel through your application, and shows a map of your application’s underlying components.

The service collects information at each execution step of your Serverless application and then makes that information available through log files and visual maps and diagrams.
- a good use case is to use it with [[lambdas|aws.svc.lambda]]. The data from X-ray can get ingested into a performance monitoring tool like Datadog, so that it gets treated like any other service in terms of APM coverage.

X-ray gets enabled at the infrastructure level (e.g. [[terraform]], [[serverless-framework]]), as well as at the application level.
