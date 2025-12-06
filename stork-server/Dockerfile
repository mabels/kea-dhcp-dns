# SPDX-License-Identifier: MPL-2.0

# This Stork docker image provides the following functionality:
# - running Stork Server service (monitoring dashboard for Kea/BIND)
# - REST API and Web UI on port 8080

FROM alpine:3.22
LABEL org.opencontainers.image.authors="Stork/Kea Developers <kea-dev@lists.isc.org>"

# Stork version - can be overridden at build time
ARG VERSION=1.19

SHELL ["/bin/ash", "-o", "pipefail", "-c"]
RUN cp /etc/apk/repositories /etc/apk/repositories_backup && \
    # Install dependencies for cloudsmith setup and Stork runtime
    apk update && apk add --no-cache curl bash tzdata ca-certificates wget && \
    # Setup Cloudsmith repo for Stork
    curl -1sLf "https://dl.cloudsmith.io/public/isc/stork/setup.alpine.sh" | bash && \
    apk update && \
    # Install Stork Server package
    apk add --no-cache "isc-stork-server~=${VERSION}" && \
    # Revert Cloudsmith repositories
    mv /etc/apk/repositories_backup /etc/apk/repositories && \
    # Remove build dependencies
    apk del curl bash

# Create directories for Stork data
RUN mkdir -p /var/lib/stork-server /usr/share/stork/www

# Volume for Stork data and certificates
VOLUME ["/var/lib/stork-server"]

# 8080 Stork Server API/UI
EXPOSE 8080/tcp

# Health check using wget (already installed for Stork)
HEALTHCHECK --interval=30s --timeout=5s --start-period=60s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/api/version || exit 1

CMD ["stork-server"]
