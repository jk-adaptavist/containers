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
    ARCH="$(uname -m)"; \
    case "$ARCH" in \
      x86_64) CLI_ARCH=amd64 ;; \
      aarch64) CLI_ARCH=arm64 ;; \
      *) echo "Unsupported architecture: $ARCH" && exit 1 ;; \
    esac; \
    wget -O influx.tar.gz "https://dl.influxdata.com/influxdb/releases/influxdb2-client-${INFLUX_CLI_VERSION}-linux-${CLI_ARCH}.tar.gz"; \
    tar -xzf influx.tar.gz; \
    mv influxdb2-client-${INFLUX_CLI_VERSION}-linux-${CLI_ARCH}/influx /usr/local/bin/; \
    rm -rf influx.tar.gz influxdb2-client-${INFLUX_CLI_VERSION}-linux-${CLI_ARCH}
    
USER nobody:nogroup