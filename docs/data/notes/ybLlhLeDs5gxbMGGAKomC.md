
### per Second
This is normally for something like Serverless Functions, where we pay for the time that the server is executing our functions. 
- ex. if we are charged $0.01 per second and the function takes 333ms to run, then the function can be executed 3 times for $0.01

### per Uptime
This is likely for container-based deployments (ex. Heroku), where we can pay per hour of uptime. This usually comes with the idea of "cold starts", where if no one is using our website, then the server will go offline (and therefore we won't get charged)
