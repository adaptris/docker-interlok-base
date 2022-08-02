FROM ghcr.io/graalvm/graalvm-ce:ol8-java11-22

# Because the arm64 builds of ol8-java11-22 appears to be bad (they're fine again 2022-01-31)
# We can replace what they're doing but doing the same thing that they should be, but with
# the correct versions?
# ARG TARGETPLATFORM
# ARG GRAAL_TARGET_JAVA=java11
# ARG GRAALVM_VERSION=22.0.0.2
# ARG GRAALVM_PKG=https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-$GRAALVM_VERSION/graalvm-ce-$GRAAL_TARGET_JAVA-GRAALVM_ARCH-$GRAALVM_VERSION.tar.gz
# RUN rm -rf /opt/graalvm-ce-$GRAAL_TARGET_JAVA-$GRAALVM_VERSION \
#     && set -eux \
#     && if [ "$TARGETPLATFORM" == "linux/amd64" ]; then GRAALVM_PKG=${GRAALVM_PKG/GRAALVM_ARCH/linux-amd64}; fi \
#     && if [ "$TARGETPLATFORM" == "linux/arm64" ]; then GRAALVM_PKG=${GRAALVM_PKG/GRAALVM_ARCH/linux-aarch64}; fi \
#     && curl --fail --silent --location --retry 3 ${GRAALVM_PKG} | gunzip | tar x -C /opt/  \
#     && java -version

ARG java_tool_opts
ENV JAVA_TOOL_OPTIONS=$java_tool_opts
ARG CURL_ARGS=-fsSL
ARG GOSU_VERSION=1.14
ARG TARGETARCH

RUN \
    java -version && \
    echo Using $TARGETARCH for gosu binary && \
    ## ol8-java-22 as -phx as the region which seems inaccessible
    echo > /etc/dnf/vars/ociregion && \
    microdnf -y update && \
    curl ${CURL_ARGS} -o /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-$TARGETARCH" && \
    chmod +x /usr/local/bin/gosu && \
    microdnf -y clean all && \
    groupadd -r interlok && \
    useradd -r -g interlok interlok && \
    gosu nobody true

COPY scripts/gosu-docker-entrypoint.sh /docker-entrypoint.sh
WORKDIR /opt/interlok
COPY builder /root/builder

RUN  \
    chmod +x /docker-entrypoint.sh && \
    cd /root/builder && \
    chmod +x /root/builder/gradlew && \
    ./gradlew --no-watch-fs --no-daemon -PinterlokDistDirectory=/opt/interlok installDist && \
    rm -rf /root/.gradle && \
    rm -rf /root/builder

ENV JVM_ARGS=-Xmx1024m
ENV JAVA_OPTS=-Dsun.net.inetaddr.ttl=30
ENV INTERLOK_OPTS=bootstrap.properties
ENV INTERLOK_BASE=/opt/interlok
ENV JAVA_TOOL_OPTIONS=""

ENTRYPOINT ["/docker-entrypoint.sh"]
