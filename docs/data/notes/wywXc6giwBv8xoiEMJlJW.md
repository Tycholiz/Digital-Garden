
A Data Lake is a single centralized repository that can accept all of our data in whatever format it comes in.

AWS implementation would be [[S3|aws.svc.S3]]

The purpose is to have all of our data in a single place so that we can perform ultra-informed analytics on the data. the alternative would be that we pull data from each data layer, aggregating it and transforming it somehow into a single format so that we could feet it into an analytics engine.
- spec: The purpose is not to have lightning-fast query speeds.

We can run different types of analytics on the data to guide better decision-making.
- ex. dashboards and visualizations, big data processing, real-time analytics, machine learning

it is often very technically involved to build and maintain a data lake.
