
A tracking pixel is also known as a *web beacon*

It is a lightweight piece of HTML that is included in a third-party website in order to grant us more visibility into how users interact with the site.

## How does it work?
The pixel itself is a 1 x 1 (ie. 1-pixel) image.
- the fact that the pixel is an `<img>` is vital. When a webpage loads and an `<img>` tag is encountered, it will send a GET request to the host company's web server (known as the *beacon web server*) to get the image. In that request is included varying identifying information about the requester, allowing the host to keep track of them.

The identifying information provided by the user's computer typically includes:
- IP address
- the time the request was made
- the type of web browser or email reader that made the request
- the existence of cookies previously sent by the host server

The host server can store all of this information, and associate it with a session identifier or tracking token that uniquely marks the interaction.

- Instead of using a Javascript-based API call, the information is sent in the parameters of the image GET request and in the HTTP headers themselves.
    - as a result, we can include the pixel in any component that supports HTML.
```html
<img src="https://myCompany.com/trackingpixel?userid=aws_user&thirdpartyname=example.hightrafficwebsite.com”>
```

### Components
- A beacon web server to receive tracking information (or a [[lambda|aws.svc.lambda]])
- A streaming engine to ingest incoming information (e.g. Aws Kinesis Data Firehose)
- A storage layer to store information retrieved from the pixel (e.g. [[S3|aws.svc.S3]])

## E Resources
- [Building a serverless tracking pixel in AWS](https://aws.amazon.com/blogs/big-data/building-a-serverless-tracking-pixel-solution-in-aws/)

{ state: State; dispatch: Dispatch }