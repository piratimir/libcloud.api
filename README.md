# libcloud.api
A dynamically constructed REST API for Apache libcloud using Flask, REST and Swagger

Load the Apache Libcloud library and turn it into a HTTP API with one simple command.

## Features

* Dynamically generate REST methods for any chosen set of drivers in libcloud
* Support for any version of libcloud
* Built in HTTP server for testing, development and prototyping
* Integrated Swagger UI for browsing documentation and JSON spec (`/api/spec`)
* Support for libcloud 'extension' method convention

## Usage

This package can either be used as a module or by using the command line wrapper.

### Command-line

You can start the API using the built in Flask development web server by running `main.py` from the command line.

```bash
python .\libcloud_api\main.py
```

### Library

The package can be included like this:

```python

from libcloud_api import libcloud_api
from libcloud_api.config import config as configuration


def main(args):
    config = configuration()
    if config.is_certificate_validation_enabled():
        import libcloud.security
        libcloud.security.VERIFY_SSL_CERT = False

    api = libcloud_api(config)
    api.build_controllers()

    # Start development server
    api.start()
```

**Options**

* `--debug, -d` - Send debug level log information to stdout

## Configuration

The `libcloud_api` class expects an instance of ``libcloud_api.config`` to be passed as an argument. This reads a YAML file, by default called config.yaml
which contains the configuration for the API.


* Providers - A list of providers - dns, storage, compute, container, backup, loadbalancer
 * Clouds - Under each provider you specify the cloud driver that you want to include, this is based on the value in ``libcloud.(compute, dns, etc.).types.Provider``
* Configuration - API specific configuration
 * Provider - Cloud - the configuration for each of the drivers that you declared in the providers section.

```yaml
providers:
  compute:
    clouds:
      - rackspace
      - dimensiondata
  backup:
    clouds:
      - dimensiondata
configuration:
  disable_certificate_validation: true
  compute:
    dimensiondata:
      api_key: "bob"
      api_secret: "blah"
```