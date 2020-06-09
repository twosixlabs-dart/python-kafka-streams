# python-kafka-streams

[![Build Status](https://github.com/twosixlabs-dart/python-kafka-streams/workflows/Build/badge.svg)](https://github.com/twosixlabs-dart/python-kafka-streams/actions)

## What Is This?

This project is a part of a collection of Python examples for working with kafka Streams via the [`Faust`](https://faust.readthedocs.io) library. This particular project contains a simple stream processor implementation. The full suite of projects are the following:

- [Producer](https://github.com/twosixlabs-dart/python-kafka-producer)
- [Stream Processor](https://github.com/twosixlabs-dart/python-kafka-streams) (this project)
- [Consumer](https://github.com/twosixlabs-dart/python-kafka-consumer)
- [Environment](https://github.com/twosixlabs-dart/kafka-examples-docker)

The *stream processor* in this example is an agent that subscribes to some topic for events. When it receives an event, it updates the payload and forwards it along to another topic. That is it!

## Getting Started

Getting started with this example requires a complete Kafka environment. [This project](https://github.com/twosixlabs-dart/kafka-examples-docker) contains a docker-compose file for setting up everything. You can use the configuration inputs to connect to a preexisting infrastructure if you have one already.

If you do not have a Python installation ready, you can configure the input and then build the Dockerfile and run the resulting image with:

```shell
docker build -t python-kafka-streams-local .
docker run --env PROGRAM_ARGS=wm-sasl-example -it python-kafka-streams-local:latest 
```


### Configuration File & SASL/SSL

The code here is configured to use JSON resources found at the subpackage `pystreams.resources.env`. Your configuration must be found within the [pystreams/resources/env](pystreams/resources/env) directory. When specifying your own you may omit the `.json` extension; it will attempt to load it as it and if that fails will attempt to load it assuming a `.json` extension. The default is to point to the [wm-sasl-example](/pystreams/resources/env/wm-sasl-example.json) configuration (which contains mostly nothing). Here is the expected format of the input file:

```json
{
    "kafka.bootstrap.servers": "",
    "auth": {
        "username": "",
        "password": ""
    },
    "app": {
        "id": "",
        "auto_offset_reset": "",
        "enable_auto_commit": false
    },
    "topic": {
        "from": "",
        "to": ""
    }
}
```

* `kafka.bootstrap.servers` - the hostname + port of the Kafka broker
* `auth`
  * `username` - username for SASL authentication
  * `password` - password for SASL authentication
* `app`
  * `id` - unique identifier for your application/group
  * `auto_offset_reset` - set to either `earliest` or `latest` to determine where a *new* app should start consuming from
  * `enable_auto_commit` - set to true to commit completed processing records
* `topic`
  * `from` - topic to consume from; currently only a single topic may be specified
  * `to` - topic to publish to; currently only a single topic may be specified

These options are subject to change/refinement, and others may be introduced in the future.
