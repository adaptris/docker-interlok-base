FROM amazoncorretto:17

LABEL maintainer="Adaptris"

ARG GOSU_VERSION=1.17
ARG GOSU_BASE_URL=https://github.com/tianon/gosu/releases/download/
ARG CURL_ARGS=-fsSL
ARG java_tool_opts
ENV JAVA_TOOL_OPTIONS=$java_tool_opts
# JAVA_HOME is set to /usr/lib/jvm/java-17-amazon-corretto which doesn't exist so we override it
ENV JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64

RUN \
    yum -y --disablerepo=AmazonCorretto update && \
    yum -y --disablerepo=AmazonCorretto install curl unzip shadow-utils gnupg2 && \
    useradd interlok && \
#    yum -y remove shadow-utils audit-libs libcap-ng libsemanage ustr && \
    yum clean all && \
    mkdir -p /opt/interlok/config && \
    mkdir -p /opt/interlok/logs && \
    mkdir -p /opt/interlok/webapps && \
    mkdir -p /opt/interlok/ui-resources

# Install, since we know corretto is x86_64; we will use the gosu-amd64 rather than try
# to map the architectures based on uname -m or something.
RUN \
    curl ${CURL_ARGS} -o /usr/local/bin/gosu -q ${GOSU_BASE_URL}/${GOSU_VERSION}/gosu-amd64;  \
    curl ${CURL_ARGS} -o /usr/local/bin/gosu.asc -q ${GOSU_BASE_URL}/${GOSU_VERSION}/gosu-amd64.asc; \
    export GNUPGHOME="$(mktemp -d)"; \
    gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4; \
    gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu; \
    rm -rf "$GNUPGHOME" /usr/local/bin/gosu.asc; \
    chmod +x /usr/local/bin/gosu; \
    gosu --version

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
