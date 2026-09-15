
### Invoke a Lambda
```sh
aws lambda invoke \
	--function-name <name/arn> \
	--payload '{ "name": "Bob" }' \
	--cli-binary-format raw-in-base64-out \
	# --invocation-type <RequestResponse (sync) | Event (async)>
	response.json
```

* * *

## Flags
Get back base64 logs from command
```sh
--log-type Tail \
```
