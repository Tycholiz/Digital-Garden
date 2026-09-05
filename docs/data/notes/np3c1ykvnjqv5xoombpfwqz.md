
## Statsd
Statsd is a protocol for capturing metrics. We communicate with the Statsd via a client library that exists in our application code. the daemon will then generate aggregate metrics and relay them to virtually any graphing or monitoring backend.

The purpopse is to instrument code.
- this can be as simple as adding a [[decorator|general.patterns.structural.decorators]] to a function that we want to time, or it might be a one-liner within the function to track a value.

The Statsd receives these calls from client libraries ([[UDP|protocol.UDP]] traffic), aggregate it over some period of time, then "flush" it to our backend (e.g. Datadog)

## Datadog Agent
The Datadog Agent is software that runs on your hosts. It collects events and metrics from hosts and sends them to Datadog

The Datadog Agents has embedded its own extended version of Statsd (DogStatsD)