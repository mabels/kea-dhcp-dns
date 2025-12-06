# Multi-arch BIND9 Docker image
# Supports ARM64 and AMD64
FROM alpine:3.22

# Install BIND9 and dependencies
# Alpine apk uses ~= for version matching (e.g., bind~=9.18 matches 9.18.x)
RUN apk add --no-cache \
    'bind~=9.18' \
    bind-tools \
    && mkdir -p /var/lib/bind/zones \
    && chown -R named:named /var/lib/bind

# Create default config directory
RUN mkdir -p /etc/bind && \
    chown -R named:named /etc/bind

# Health check
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD dig @localhost -p 5353 version.bind chaos txt || exit 1

# Run as named user
USER named

# Default command
CMD ["/usr/sbin/named", "-c", "/etc/bind/named.conf", "-g"]

# Expose DNS ports
EXPOSE 5353/tcp 5353/udp
