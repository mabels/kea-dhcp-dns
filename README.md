# Kea DHCP Multi-Arch Container Images

This repository builds multi-architecture (x86_64 and ARM64) container images for ISC Kea DHCP server and DDNS components using the [official ISC Kea Docker repository](https://gitlab.isc.org/isc-projects/kea-docker).

## Images

- `kea-dhcp4`: Kea DHCPv4 server
- `kea-dhcp6`: Kea DHCPv6 server
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
docker pull ghcr.io/mabels/kea-dhcp-dns/kea-dhcp4:latest
docker pull ghcr.io/mabels/kea-dhcp-dns/kea-dhcp6:latest
docker pull ghcr.io/mabels/kea-dhcp-dns/kea-dhcp-ddns:latest
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

**Automatically detected** from the latest stable release tag in the [ISC Kea GitLab repository](https://gitlab.isc.org/isc-projects/kea).

The workflow automatically fetches the latest stable release (excluding alpha, beta, and rc versions) from the Kea project tags on every build. No manual version configuration needed!

## How It Works

The workflow:
1. Checks out the official [ISC kea-docker repository](https://gitlab.isc.org/isc-projects/kea-docker)
2. Uses their proven Dockerfiles (Alpine Linux + Cloudsmith packages)
3. Builds for both `linux/amd64` and `linux/arm64` using QEMU emulation
4. Publishes multi-arch images to GitHub Container Registry

This approach ensures:
- Always using official, tested Dockerfiles
- Automatic updates when ISC updates their Dockerfiles
- Native Alpine package support for both architectures
- No custom Dockerfile maintenance needed

## Local Build

To build locally, first clone the ISC kea-docker repository:

```bash
git clone https://gitlab.isc.org/isc-projects/kea-docker.git
cd kea-docker/kea-dhcp4
docker build --build-arg VERSION=2.6 -t kea-dhcp4:latest .
```
