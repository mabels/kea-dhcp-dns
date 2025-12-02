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

The workflow builds multi-arch images using QEMU emulation on GitHub Actions:
- **Standard GitHub runners** (`ubuntu-latest`)
- **QEMU emulation** for ARM64 cross-compilation
- **Docker Buildx** for multi-platform builds
- Single job builds both `linux/amd64` and `linux/arm64` platforms simultaneously

## Local Build

To build locally:

```bash
# Build kea-dhcp4 for your architecture
docker build -f Dockerfile.kea-dhcp4 -t kea-dhcp4:latest .

# Build kea-dhcp-ddns for your architecture
docker build -f Dockerfile.kea-dhcp-ddns -t kea-dhcp-ddns:latest .
```
