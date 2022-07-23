
A deployment artifact is synonymous with a *build*: it is the application code as it runs in production (compiled, built, bundled, minified, and optimized)
- most often it's a single binary, or bunch of files compressed in an archive.
	- due to this nature, artifacts can be stored and versioned.

Ultimate goal of an artifact is for it to be downloaded as fast as possible on a server, and run immediately.

An artifact should be configurable so it can be deployed on any environment. Only the configuration (ie. env variables) changes.
- ie. we need to be able to deploy the same artifact to both staging and production servers.
	- if you have to build the artifact 2 times for staging and production, you are doing it wrong.

Typically, an artifact is either stored in an S3 bucket or it is deployed to an external server.

The artifact-based workflow gives us advantages:
- quick to deploy
- instant to rollback
- backups are readily available
- if a build works on staging, the only reason it wouldn't work on production is because of the configuration (or the environments aren't the same, which shouldn't be the case).
- feature flagging is easier, since we just need to check the env variables.
