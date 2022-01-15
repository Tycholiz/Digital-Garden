
# S3 (Simple Storage Service)
Fundamentally, you can think of S3 as a highly-durable hash table in the cloud.
- The key can be any string, and
- the value any blob/object of data up to 5 TB.

Object storage is used for purposes such as storing photos on Facebook, songs on Spotify, or files in online collaboration services, such as Dropbox.

Data gets streamed at a rate of around 90 MB/s.

You can have as many parallel uploads and downloads as you want, thus, the infinite bandwidth.

S3 offers built-in redundancy.

At first you can start with the default storage class and ignore all the other classes. Don't bother with the implications of the different storage classes until you really need to start saving money from S3 storage costs.

### Limitations
- cannot append to objects.
- It doesn’t support HTTPS when used as a static website host, so you cannot host static websites

- Alternatives: Azure Blob

Pricing
$23.55/TB/month
$5/million uploads and $0.40/million downloads
