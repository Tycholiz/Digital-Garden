
Fundamentally, you can think of S3 as a highly-durable hash table in the cloud.
- The key can be any string, and
- the value any blob/object of data up to 5 TB.

Object storage is used for purposes such as storing photos on Facebook, songs on Spotify, or files in online collaboration services, such as Dropbox.

When using S3, always consider implementing it alongside a CDN like [[CloudFront|aws.svc.cloudfront]]
- ex. if you were building a video streaming platform, you would want to cache egress data so that 2 users living in a similar geographical location would be able to benefit from content that is cached at the edge.

Data gets streamed at a rate of around 90 MB/s.

You can have as many parallel uploads and downloads as you want, thus, the infinite bandwidth.

S3 offers built-in redundancy.

At first you can start with the default storage class and ignore all the other classes. Don't bother with the implications of the different storage classes until you really need to start saving money from S3 storage costs.

### Event Notifications
Like webhooks, allows us to receive notifications when certain events happen in your S3 bucket.

### Limitations
- cannot append to objects.
- It doesn’t support HTTPS when used as a static website host, so you cannot host static websites

* * *

- Alternatives: Azure Blob

Pricing
$23.55/TB/month
$5/million uploads and $0.40/million downloads
