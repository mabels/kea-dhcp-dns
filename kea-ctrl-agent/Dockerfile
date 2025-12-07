# Multi-arch Kea Control Agent Docker image
# Supports ARM64 and AMD64
FROM alpine:3.22

ARG VERSION=3.0

SHELL ["/bin/ash", "-o", "pipefail", "-c"]
RUN cp /etc/apk/repositories /etc/apk/repositories_backup && \
    # Install curl and bash for cloudsmith's script. Tzdata to allow timezone change.
    apk update && apk add --no-cache curl bash tzdata && \
    # Setup Cloudsmith repo
    if test "$(expr "$(echo "${VERSION}" | cut -d '.' -f 2)" % 2)" = 0; then \
        repo="kea-$(echo "${VERSION}" | cut -d '.' -f 1)-$(echo "${VERSION}" | cut -d '.' -f 2)"; \
    else \
        repo='kea-dev'; \
    fi && \
    curl -1sLf "https://dl.cloudsmith.io/public/isc/${repo}/setup.alpine.sh" | bash && \
    apk update && \
    # Install kea-ctrl-agent package
    apk add --no-cache \
        isc-kea-ctrl-agent~=${VERSION} && \
    # Revert Cloudsmith repositories
    mv /etc/apk/repositories_backup /etc/apk/repositories && \
    # remove curl and bash
    apk del curl bash

VOLUME ["/etc/kea"]

# 8000 command control channel
EXPOSE 8000/tcp

CMD ["/usr/sbin/kea-ctrl-agent", "-c", "/etc/kea/kea-ctrl-agent.conf"]
