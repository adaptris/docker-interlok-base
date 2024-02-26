FROM azul/zulu-openjdk-alpine:11

LABEL maintainer="Adaptris"

ARG java_tool_opts
ENV JAVA_TOOL_OPTIONS=$java_tool_opts

RUN \
    apk add --no-cache --update ca-certificates bash curl unzip su-exec && \
    addgroup -S interlok && \
    adduser -S interlok -G interlok && \
    mkdir -p /opt/interlok/config && \
    mkdir -p /opt/interlok/logs && \
    mkdir -p /opt/interlok/webapps && \
    mkdir -p /opt/interlok/ui-resources

COPY scripts/suexec-docker-entrypoint.sh /docker-entrypoint.sh
WORKDIR /opt/interlok

RUN \
    chmod +x /docker-entrypoint.sh

ENV JVM_ARGS=-Xmx1024m
ENV JAVA_OPTS=-Dsun.net.inetaddr.ttl=30
ENV INTERLOK_OPTS=bootstrap.properties
ENV INTERLOK_BASE=/opt/interlok
ENV JAVA_TOOL_OPTIONS=""

ENTRYPOINT ["/docker-entrypoint.sh"]
