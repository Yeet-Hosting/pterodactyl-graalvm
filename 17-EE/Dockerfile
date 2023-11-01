# ----------------------------------
# Pterodactyl Core Dockerfile
# Environment: Java
# Minimum Panel Version: 1.7.0
# ----------------------------------
FROM alpine:3.14

ARG TARGETPLATFORM
ARG GRAAL_VERSION=22.3.2
ARG JAVA_VERSION=17

MAINTAINER RikoDEV, <kontakt@riko.dev>

#ENV DEBIAN_FRONTEND=noninteractive

# Default to UTF-8 file.encoding
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

RUN apk update && \
    apk add --no-cache curl ca-certificates openssl git tar sqlite fontconfig tzdata iproute2 && \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen && \
    case ${TARGETPLATFORM} in \
        "linux/amd64")  ARCH=linux-amd64  ;; \
        "linux/arm64")  ARCH=linux-aarch64  ;; \
    esac && \
    curl --retry 3 -Lfso /tmp/graalvm.tar.gz https://oca.opensource.oracle.com/gds/GRAALVM_EE_JAVA17_22_3_2/graalvm-ee-java17-${ARCH}-22.3.2.tar.gz && \
    mkdir -p /opt/java/graalvm && \
    cd /opt/java/graalvm && \
    tar -xf /tmp/graalvm.tar.gz --strip-components=1 && \
    export PATH="/opt/java/graalvm/bin:$PATH" && \
    rm -rf /tmp/graalvm.tar.gz

ENV JAVA_HOME=/opt/java/graalvm \
    PATH="/opt/java/graalvm/bin:$PATH"

# Step 2 - add pterodactyl stuff
RUN adduser -D -h /home/container container

USER        container
ENV         USER=container HOME=/home/container

WORKDIR     /home/container

COPY        ./entrypoint.sh /entrypoint.sh

CMD         ["/bin/sh", "/entrypoint.sh"]
