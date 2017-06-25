# docker-statsd

This repository contains a templated `Dockerfile` for image variants designed to support StatsD. The Librato backend is installed as well.

## Usage

### Basic Usage

In order to supply a custom configuration to the StatsD daemon, you can either volume mount or `COPY` a custom `config.js` into the correct file system location, or override `CMD` so that it points to your custom configuration file:

```bash
$ docker run -v config.js:/var/lib/statsd/config.js quay.io/azavea/statsd:0.8-alpine
```

Or define your own `Dockerfile` with:

```dockerfile
FROM quay.io/azavea/statsd:0.8-alpine

COPY config.js /var/lib/statsd/config.js
```

### Librato Backend

In order to make use of the StatsD backend for Librato, override the container's `ENTRYPOINT` to:

```dockerfile
ENTRYPOINT ["node_modules/.bin/statsd-librato"]
```

Also, the following environment variables are parsed by the `statsd-librato` binary packaged with the backend:

- `LIBRATO_EMAIL` - Librato email address
- `LIBRATO_TOKEN` - Librato API token
- `LIBRATRO_SOURCE` - Environment metrics are originating from

**Note**: `LIBRATRO_SOURCE` above is not a typo in the `README`, it is a typo in the environment variables reference in `statsd-librato`. We will update it when it is fixed in an upstream release. 

### Template Variables

- `STATSD_VERSION` - StatsD daemon version
- `STATSD_LIBRATO_BACKEND_VERSION` - StatsD Librato backend version
- `VARIANT` - Base container image variant

### Testing

An example of how to use `cibuild` to build and test an image:

```bash
$ CI=1 STATSD_VERSION=0.8.0 STATSD_LIBRATO_BACKEND_VERSION=2.0.13 \
  VARIANT=alpine \
  ./scripts/cibuild
```
