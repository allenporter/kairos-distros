# renovate: datasource=github-releases depName=kairos-io/kairos
ARG KAIROS_VERSION=v2.4.3

# Note currently renovated since it depends on Kairos support
ARG K3S_VERSION=v1.28.2+k3s1
ARG UBUNTU_VERSION=22.04

# Full version is something like v2.4.3-k3sv1.28.2-k3s1
ARG KAIROS_FULL_VERSION="${KAIROS_VERSION}-${K3S_VERSION}"

FROM quay.io/kairos/ubuntu:${UBUNTU_VERSION}-standard-amd64-generic-${KAIROS_FULL_VERSION}

COPY rootfs/ /

RUN apt-get update && \
    apt-get install -y \
    wget=1.21.* \
    nvidia-container-toolkit=1.14.3-1 \
    && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Configure nvidia container runtime
RUN nvidia-ctk runtime configure --runtime=containerd