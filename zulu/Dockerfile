FROM azul/zulu-openjdk:8

LABEL maintainer="Adaptris"

ARG INTERLOK_VERSION=3.10.0
ARG BASE_URL=https://development.adaptris.net/installers/interlok/$INTERLOK_VERSION
ARG java_tool_opts
ENV JAVA_TOOL_OPTIONS=$java_tool_opts

RUN apt-get -y -q update && \
    apt-get install --no-install-recommends -y curl bash unzip ca-certificates gosu && \
    groupadd -r interlok && \
    useradd -r -g interlok interlok && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /opt/interlok/logs && \
    gosu nobody true

COPY scripts/gosu-docker-entrypoint.sh /docker-entrypoint.sh
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
