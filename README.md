# Docker Interlok Base

[![Docker Publish](https://github.com/adaptris/docker-interlok-base/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/adaptris/docker-interlok-base/actions/workflows/docker-publish.yml)
[![Docker Pulls](https://img.shields.io/docker/pulls/adaptris/interlok-base.svg)](https://hub.docker.com/r/adaptris/interlok-base/)

[https://hub.docker.com/r/adaptris/interlok](https://hub.docker.com/r/adaptris/interlok-base)

Base Docker image ready to install Interlok

* interlok user created.
* Dir /opt/interlok and sub dirs config, logs, webapps and ui-resources created.
* Various distribution platforms are defined in the corresponding sub directories.

## Notes

This image does not have any instance of Interlok installed. This is a base to build your own docker image.

Our suggestions would be to

* Expose ports 8080 + 5555
* Volumes :
    * /opt/interlok/config -> for layering in your own configuration
    * /opt/interlok/logs  -> for logging; if you're using log4j logging to diskk
    * /opt/interlok/ui-resources -> for preserving templates and saved projects.

## Example:

Considering that you have an Interlok config project assembled to an instance in `./build/distribution`.

Dockerfile:

```
FROM adaptris/interlok-base:latest-debian

COPY ./build/distribution /opt/interlok

EXPOSE 8080 5555
VOLUME [ "/opt/interlok/config", "/opt/interlok/logs" , "/opt/interlok/ui-resources" ]
```
