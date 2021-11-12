
if Caddy is running at port `:2019`, then the config file is at `:2019/config/`

most(?) caddy commands are just running GET/POST requests. For instance, `caddy stop` sends a POST to `http://localhost:2019/stop`

#### Workflow
To administer Caddy, we can do so in 2 ways:
1. using the API
    - ex. POST to `https://caddyserver.mywebsite.com:443/load`
2. using the CLI
    - ex. run `caddy load`

Caddyfile+CLI combo is the more easy-going route, but is more difficult to scale. Otherwise, if looking for more control, users typically go for the JSON+API combo.