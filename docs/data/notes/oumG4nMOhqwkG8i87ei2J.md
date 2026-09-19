
## Challenges
- PubSub is inherently asynchronous. Your test can’t just make an API request and then perform an assertion on the response.
- EventBridge has no persistent storage of events that you can query via its API.
- How do you control the side effects caused by multiple downstream subscribers when publishing to EventBridge from your tests?

We want to test for 3 main scenarios:
- Publisher fails to send event to EventBridge
- Subscriber does not receive event that it should have
- Subscriber receives event but fails to process it correctly

## E Resources
[Test that EventBridge event was fired](https://stackoverflow.com/questions/60743785/test-that-an-aws-eventbridge-or-cloudwatch-event-was-fired)
