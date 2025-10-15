# syntax=docker/dockerfile:1

ARG VERSION
FROM docker.io/library/alpine:3.22

USER root

RUN \
    apk add --no-cache \
        bash \
        ca-certificates \
        curl \
        jq \
        tzdata \
        gnupg \
        tar \
    && rm -rf /tmp/*
# Install the influx CLI
RUN set -eux; \
    arch="$(uname -m)"; \
    case "$arch" in \
       x86_64) export ARCH=amd64 ;; \
       aarch64) export ARCH=arm64 ;; \
       *) echo "Unsupported arch: $arch" && exit 1 ;; \
    esac; \
    export GNUPGHOME="$(mktemp -d)"; \
    gpg --batch --keyserver keyserver.ubuntu.com --recv-keys \
      24C975CBA61A024EE1B631787C3D57159FC2F927; \
    curl -fLO "https://dl.influxdata.com/influxdb/releases/influxdb2-client-${INFLUX_CLI_VERSION}-linux-${ARCH}.tar.gz" \
         -fLO "https://dl.influxdata.com/influxdb/releases/influxdb2-client-${INFLUX_CLI_VERSION}-linux-${ARCH}.tar.gz.asc"; \
    gpg --batch --verify "influxdb2-client-${INFLUX_CLI_VERSION}-linux-${ARCH}.tar.gz.asc" \
                         "influxdb2-client-${INFLUX_CLI_VERSION}-linux-${ARCH}.tar.gz"; \
    tar xzf "influxdb2-client-${INFLUX_CLI_VERSION}-linux-${ARCH}.tar.gz" -C /usr/local/bin ./influx; \
    rm -rf /tmp/* "$GNUPGHOME" \
           "influxdb2-client-${INFLUX_CLI_VERSION}-linux-${ARCH}.tar.gz" \
           "influxdb2-client-${INFLUX_CLI_VERSION}-linux-${ARCH}.tar.gz.asc"
 
    
USER nobody:nogroup