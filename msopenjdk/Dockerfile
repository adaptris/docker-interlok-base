FROM mcr.microsoft.com/openjdk/jdk:17-ubuntu

LABEL maintainer="Adaptris"

ARG java_tool_opts
ENV JAVA_TOOL_OPTIONS=$java_tool_opts

RUN \
    apt-get -y -q update && \
    apt-get install --no-install-recommends -y curl bash unzip ca-certificates gosu && \
    groupadd -r interlok && \
    useradd -r -g interlok interlok && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /opt/interlok/config && \
    mkdir -p /opt/interlok/logs && \
    mkdir -p /opt/interlok/webapps && \
    mkdir -p /opt/interlok/ui-resources && \
    gosu nobody true

COPY scripts/gosu-docker-entrypoint.sh /docker-entrypoint.sh
WORKDIR /opt/interlok

RUN \
    chmod +x /docker-entrypoint.sh

ENV JVM_ARGS=-Xmx1024m
ENV JAVA_OPTS=-Dsun.net.inetaddr.ttl=30
ENV INTERLOK_OPTS=bootstrap.properties
ENV INTERLOK_BASE=/opt/interlok
ENV JAVA_TOOL_OPTIONS=""

ENTRYPOINT ["/docker-entrypoint.sh"]
