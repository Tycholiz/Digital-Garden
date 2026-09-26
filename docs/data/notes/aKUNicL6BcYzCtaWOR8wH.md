
### `docker images`
- show all images

### `docker build`
- purpose is to build an image from a Dockerfile. The command will find the `Dockerfile` and will build an image based on it.

### Inject env variables into the Dockerfile build process
`--build-arg NPM_TOKEN=$NPM_TOKEN`

## Build image
- from your application folder:
`docker build -t <APP NAME> .`
  - this causes each line of Dockerfile to be executed, building up the image as it goes
- Images are static once they are created
