# renovate: datasource=github-releases depName=kairos-io/kairos
ARG KAIROS_VERSION=v2.5.0

# Note currently renovated since it depends on Kairos support
ARG K3S_VERSION=v1.28.2-k3s1
ARG UBUNTU_VERSION=22.04

# Full version is something like v2.4.3-k3sv1.28.2-k3s1
ARG KAIROS_FULL_VERSION="${KAIROS_VERSION}-k3s${K3S_VERSION}"
ARG BASE_IMAGE=quay.io/kairos/ubuntu:${UBUNTU_VERSION}-standard-amd64-generic-${KAIROS_FULL_VERSION}
ARG IMAGE_VARIANT="ubuntu${UBUNTU_VERSION}"

FROM ${BASE_IMAGE}
ARG IMAGE_VARIANT

RUN curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
RUN curl -s -L https://nvidia.github.io/libnvidia-container/${IMAGE_VARIANT}/libnvidia-container.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

RUN apt-get update && \
    apt-get install -y \
        nvidia-container-toolkit=1.14.3-1 \
        && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Configure nvidia container runtime
RUN nvidia-ctk runtime configure --runtime=containerd