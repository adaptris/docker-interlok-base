FROM openjdk:8-jre-slim

LABEL maintainer="Adaptris"

ARG BASE_URL=https://development.adaptris.net/installers/Interlok/latest-stable/
ARG INTERLOK_VERSION=3.10-SNAPSHOT
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
COPY builder /root/builder

RUN  \
    curl -fsSL -o base-filesystem.zip -q ${BASE_URL}/base-filesystem.zip && \
    curl -fsSL -o runtime-libraries.zip ${BASE_URL}/runtime-libraries.zip && \
    unzip -o -q  runtime-libraries.zip && \
    unzip -o -q  base-filesystem.zip && \
    rm -rf /opt/interlok/optional /opt/interlok/lib/interlok-flatfile.jar /opt/interlok/lib/interlok-mail.jar && \
    chmod +x /docker-entrypoint.sh && \
    rm -rf *.zip && \
    cd /root/builder && \
    chmod +x /root/builder/gradlew && \
    ./gradlew --no-daemon -PinterlokCoreVersion=${INTERLOK_VERSION} installDist && \
    rm -rf /root/.gradle && \
    rm -rf /root/builder

ENV JVM_ARGS=-Xmx1024m
ENV JAVA_OPTS=-Dsun.net.inetaddr.ttl=30
ENV INTERLOK_OPTS=bootstrap.properties
ENV INTERLOK_BASE=/opt/interlok
ENV JAVA_TOOL_OPTIONS=""

ENTRYPOINT ["/docker-entrypoint.sh"]
