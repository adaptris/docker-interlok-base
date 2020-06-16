FROM azul/zulu-openjdk-alpine:8

LABEL maintainer="Adaptris"

ARG INTERLOK_VERSION=3.10.1
ARG BASE_URL=https://development.adaptris.net/installers/interlok/$INTERLOK_VERSION
ARG java_tool_opts
ENV JAVA_TOOL_OPTIONS=$java_tool_opts
# JAVA_HOME is set wrong on the latest 8-jre image.
ENV JAVA_HOME=/usr/lib/jvm/zulu8

RUN apk add --no-cache --update ca-certificates bash curl unzip su-exec && \
    addgroup -S interlok && \
    adduser -S interlok -G interlok && \
    mkdir -p /opt/interlok/logs

COPY scripts/suexec-docker-entrypoint.sh /docker-entrypoint.sh
WORKDIR /opt/interlok

RUN  \
    curl -fsSL -o base-filesystem.zip -q ${BASE_URL}/base-filesystem.zip && \
    curl -fsSL -o runtime-libraries.zip -q ${BASE_URL}/runtime-libraries.zip && \
    curl -fsSL -o javadocs.zip ${BASE_URL}/javadocs.zip && \
    unzip -o -q javadocs.zip && \
    unzip -o -q  base-filesystem.zip && \
    unzip -o -q runtime-libraries.zip && \
    rm -rf /opt/interlok/optional && \
    rm -rf /opt/interlok/docs/javadocs/optional && \
    chmod +x /docker-entrypoint.sh && \
    rm -rf *.zip

ENV JVM_ARGS=-Xmx1024m
ENV JAVA_OPTS=-Dsun.net.inetaddr.ttl=30
ENV INTERLOK_OPTS=bootstrap.properties
ENV INTERLOK_BASE=/opt/interlok
ENV JAVA_TOOL_OPTIONS=""

ENTRYPOINT ["/docker-entrypoint.sh"]