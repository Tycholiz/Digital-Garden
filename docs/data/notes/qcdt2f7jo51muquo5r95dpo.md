
Kibana enables you to interactively explore, visualize, and share insights into your data and manage and monitor the stack.

To use Kibana, we need to pass an enrollment token that is provided from the ElasticSearch server
- this enrollment token is output to the log the first time we run the ES server.

## Using with Docker
[guide](https://www.elastic.co/guide/en/kibana/current/docker.html)

- You may need to mount a [[volume|docker.containers.volumes]] on the kibana container in order to access a `kibana.yml` locally
    - give it contents:
```yml
server.host: "0.0.0.0"
```

- if Kibana web portal is stuck "configuring" forever, just reload the URL and enter username/password