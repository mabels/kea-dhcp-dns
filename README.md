# Kea DHCP Multi-Arch Container Images

This repository builds multi-architecture (x86_64 and ARM64) container images for ISC Kea DHCP server and DDNS components.

## Images

- `kea-dhcp4`: Kea DHCP4 server
- `kea-dhcp-ddns`: Kea DHCP Dynamic DNS

## Platforms

- `linux/amd64` (x86_64)
- `linux/arm64` (ARM64/aarch64)

## Build Schedule

Images are automatically built:
- Weekly on Mondays at 02:00 UTC
- On every push to main/master branch
- Can be manually triggered via GitHub Actions

## Usage

The images are published to GitHub Container Registry:

```bash
docker pull ghcr.io/<your-username>/kea-dhcp-ddns/kea-dhcp4:latest
docker pull ghcr.io/<your-username>/kea-dhcp-ddns/kea-dhcp-ddns:latest
```

## Configuration

Mount your configuration files to `/etc/kea/`:

```yaml
volumeMounts:
  - name: config
    mountPath: /etc/kea/kea-dhcp4.conf
    subPath: kea-dhcp4.conf
```

## Kea Version

Current version: **2.6.1**

To update the Kea version, modify the `KEA_VERSION` environment variable in `.github/workflows/build.yaml`.

## Architecture

The workflow builds on **native runners** for maximum performance:
- **AMD64 builds** run on standard `ubuntu-latest` runners
- **ARM64 builds** run on `ubuntu-24.04-arm64` runners
- No QEMU emulation is used, resulting in faster and more reliable builds

After both architecture-specific builds complete, a multi-arch manifest is created that combines them into a single image tag.

## Local Build

To build locally:

```bash
# Build kea-dhcp4 for your architecture
docker build -f Dockerfile.kea-dhcp4 -t kea-dhcp4:latest .

# Build kea-dhcp-ddns for your architecture
docker build -f Dockerfile.kea-dhcp-ddns -t kea-dhcp-ddns:latest .
```
