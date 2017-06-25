# docker-statsd

This repository contains a templated `Dockerfile` for image variants designed to support StatsD.

## Usage

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
