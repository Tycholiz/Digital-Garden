
## Terminology
### Sources
A source is any asset that collects data.
- ex. our WebApp client
- it's worth defining each environment as different sources (e.g. WebApp Client Dev, WebApp Client Staging etc.), which allows for flexibility in directing analytics traffic to destinations.

### Destinations
A destination is an integration that has been established by Segment
- ex. Google Analytics, FB Pixel, Google Ads, Amplitude, AWS Redshift
- If we have elected to have one source per environment (as stated above), then we should also have a destination per environment.

Destinations prevent us from having to implement multiple destination SDKs within the codebase.

## API
### `Identify`
When a user starts out on your site, they are anonymous and are given an `anonymousId`
The `Identify` call lets you tie a user to their actions and record traits about them. It includes a unique User ID and any optional traits you know about the user, like their email, name, and more.

We should make an `Identify` call:
- After a user first registers
- After a user logs in
- When a user updates their info (for example, they change or add a new address)

Typical identify call:
```js
analytics.identify("97980cfea0067", {
  name: "Peter Gibbons",
  email: "peter@example.com",
  plan: "premium",
  logins: 5
});
```
Which would result in the following payload:
```json
{
  "type": "identify",
  "traits": {
    "name": "Peter Gibbons",
    "email": "peter@example.com",
    "plan": "premium",
    "logins": 5
  },
  "userId": "97980cfea0067"
}
```

### `Page`
Page calls track the web page that a user visits. 
- By default, Segment sends the following properties `title`, `path` , `url`, `referrer`, and `search`.

### `Track`
Track calls record user actions which are also known as events. 
- They mainly consist of an event name and a user-defined properties object.
- event names should be human readable, and built from a noun and past tense verb. 
  - ex. `Appointment Request Submitted`