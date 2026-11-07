
Environment variables can be
- `self` - defined within the same yml file
- `env` - defined within an `env` file
- `opt` - defined on CLI
  - ex. `sls invoke --stage="local" ...` can be accessed with `${opt:stage}`

## Resources
- https://www.serverless.com/framework/docs/providers/aws/guide/variables
- [3 ways to access envronment variables in serverless app](https://dev.to/eoinsha/3-ways-to-read-ssm-parameters-4555)