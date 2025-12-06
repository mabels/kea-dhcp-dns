# SPDX-License-Identifier: MPL-2.0

# This Stork docker image provides the following functionality:
# - running Stork Agent service (monitoring agent for Kea/BIND)
# - Reports metrics to Stork Server via gRPC on port 8080

FROM alpine:3.22
LABEL org.opencontainers.image.authors="Stork/Kea Developers <kea-dev@lists.isc.org>"

# Stork version - can be overridden at build time
ARG VERSION=1.19

SHELL ["/bin/ash", "-o", "pipefail", "-c"]
RUN cp /etc/apk/repositories /etc/apk/repositories_backup && \
    # Install dependencies for cloudsmith setup and Stork runtime
    apk update && apk add --no-cache curl bash tzdata ca-certificates procps && \
    # Setup Cloudsmith repo for Stork
    curl -1sLf "https://dl.cloudsmith.io/public/isc/stork/setup.alpine.sh" | bash && \
    apk update && \
    # Install Stork Agent package
    apk add --no-cache "isc-stork-agent~=${VERSION}" && \
    # Revert Cloudsmith repositories
    mv /etc/apk/repositories_backup /etc/apk/repositories && \
    # Remove build dependencies
    apk del curl bash

# Create directories for Stork agent data
RUN mkdir -p /var/lib/stork-agent

# Volume for Stork agent data and certificates
VOLUME ["/var/lib/stork-agent"]

# 8080 Stork Agent gRPC
EXPOSE 8080/tcp

# Health check using procps (pgrep)
HEALTHCHECK --interval=30s --timeout=5s --start-period=60s --retries=3 \
    CMD pgrep -f stork-agent || exit 1

CMD ["stork-agent"]
