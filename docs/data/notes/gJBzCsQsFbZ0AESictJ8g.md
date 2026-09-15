
CloudWatch is a monitoring and observability service.

CloudWatch collects monitoring and operational data in the form of logs, metrics, and events.

Purpose is to get a unified view of operational health and gain complete visibility of your AWS resources, applications, and services running on AWS and on-premises

CloudWatch can be set up to...
- detect anomalous behavior in your environments
- set alarms
- visualize logs and metrics side by side
- take automated actions
- troubleshoot issue
- discover insights

## Cloudwatch Logs
CloudWatch Logs enables you to centralize the logs from all of your systems, applications, and [[AWS Services|aws.svc]]
monitor, store, and access your log files from

### Log Event
A log event is a record of some activity recorded by the application or resource being monitored.

Contains two properties: the timestamp of when the event occurred, and the raw event message.

### Log Stream
A log stream is a sequence of log events that share the same source. Each separate source of logs in CloudWatch Logs makes up a separate log stream.

a log stream is generally intended to represent the sequence of events coming from the application instance or resource being monitored

### Log Group
A log group is a group of log streams that share the same retention, monitoring, and access control settings.
- You can define log groups and specify which streams to put into each group.

## Concepts

### Namespaces
A namespace is a container for CloudWatch metrics
- Metrics in different namespaces are isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics.

There is no default namespace. You must specify a namespace for each data point you publish to CloudWatch.
- namespaces typically use the following naming convention: AWS/service. For example, Amazon EC2 uses the AWS/EC2 namespace

### Metrics
A metric represents a time-ordered set of data points that are published to CloudWatch.
- Think of a metric as a variable to monitor, and the data points as representing the values of that variable over time.
- ex. the CPU usage of a particular EC2 instance is one metric provided by Amazon EC2. The data points themselves can come from any application or business activity from which you collect data.

Metrics are uniquely defined by a name, a namespace, and zero or more dimensions.

#### Resolution
Each metric is one of the following:
- Standard resolution, with data having a one-minute granularity
- High resolution, with data at a granularity of one second

Metrics produced by AWS services are standard resolution by default.

When you publish a high-resolution metric, CloudWatch stores it with a resolution of 1 second, and you can read and retrieve it with a period of 1 second, 5 seconds, 10 seconds, 30 seconds, or any multiple of 60 seconds.

High-resolution metrics can give you more immediate insight into your application's sub-minute activity.

### Dimension
A dimension is a name/value pair that is part of the identity of a metric. You can assign up to 10 dimensions to a metric.

Every metric has specific characteristics that describe it, and you can think of dimensions as categories for those characteristics. Dimensions help you design a structure for your statistics plan. Because dimensions are part of the unique identifier for a metric, whenever you add a unique name/value pair to one of your metrics, you are creating a new variation of that metric.

AWS services that send data to CloudWatch attach dimensions to each metric.
- You can use dimensions to filter the results that CloudWatch returns.
	- ex. you can get statistics for a specific EC2 instance by specifying the InstanceId dimension when you search for metrics.

CloudWatch treats each unique combination of dimensions as a separate metric, even if the metrics have the same metric name.

suppose that you publish four distinct metrics named ServerStats in the DataCenterMetric namespace with the following properties:

```
Dimensions: Server=Prod, Domain=Frankfurt, Unit: Count, Timestamp: 2016-10-31T12:30:00Z, Value: 105
Dimensions: Server=Beta, Domain=Frankfurt, Unit: Count, Timestamp: 2016-10-31T12:31:00Z, Value: 115
Dimensions: Server=Prod, Domain=Rio,       Unit: Count, Timestamp: 2016-10-31T12:32:00Z, Value: 95
Dimensions: Server=Beta, Domain=Rio,       Unit: Count, Timestamp: 2016-10-31T12:33:00Z, Value: 97
```

If you publish only those four metrics, you can retrieve statistics for these combinations of dimensions:
- `Server=Prod,Domain=Frankfurt`
- `Server=Prod,Domain=Rio`
- `Server=Beta,Domain=Frankfurt`
- `Server=Beta,Domain=Rio`

You can't retrieve statistics for the following dimensions or if you specify no dimensions.
- `Server=Prod`
- `Server=Beta`
- `Domain=Frankfurt`
- `Domain=Rio`

### Alarms
You can use an alarm to automatically initiate actions on your behalf. An alarm watches a single metric over a specified time period, and performs one or more specified actions, based on the value of the metric relative to a threshold over time.

You can create alarms that watch metrics and send notifications or automatically make changes to the resources you are monitoring when a threshold is breached. For example, you can monitor the CPU usage and disk reads and writes of your Amazon EC2 instances and then use this data to determine whether you should launch additional instances to handle increased load. You can also use this data to stop under-used instances to save money.
