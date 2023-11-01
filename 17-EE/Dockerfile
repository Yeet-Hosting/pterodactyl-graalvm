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
RUN apk add tzdata
RUN cp /usr/share/zoneinfo/Europe/Berlin /etc/localtime
RUN rm -r /usr/share/zoneinfo/Africa && \
    rm -r /usr/share/zoneinfo/Antarctica && \
    rm -r /usr/share/zoneinfo/Arctic && \
    rm -r /usr/share/zoneinfo/Asia && \
    rm -r /usr/share/zoneinfo/Atlantic && \
    rm -r /usr/share/zoneinfo/Australia && \
    rm -r /usr/share/zoneinfo/America  && \
    rm -r /usr/share/zoneinfo/Indian && \
    rm -r /usr/share/zoneinfo/Mexico && \
    rm -r /usr/share/zoneinfo/Pacific && \
    rm -r /usr/share/zoneinfo/Chile && \
    rm -r /usr/share/zoneinfo/Canada
RUN echo "Europe/Berlin" >  /etc/timezone

ENV TZ Europe/Berlin
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

RUN apk update && \
    apk add --no-cache curl ca-certificates openssl git tar sqlite fontconfig tzdata iproute2 && \
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
