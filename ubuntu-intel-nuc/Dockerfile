# renovate: datasource=github-releases depName=kairos-io/kairos
ARG KAIROS_VERSION=v2.4.3

# Note currently renovated since it depends on Kairos support
ARG K3S_VERSION=v1.28.2+k3s1
ARG UBUNTU_VERSION=22.04

# Full version is something like v2.4.3-k3sv1.28.2-k3s1
ENV KAIROS_FULL_VERSION="${KAIROS_VERSION}-k3s${K3S_VERSION//-/+}"

FROM quay.io/kairos/ubuntu:${UBUNTU_VERSION}-standard-amd64-generic-${KAIROS_FULL_VERSION}

RUN apt-get update && \
    apt-get install -y \
    wget=1.21.* \
    && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# renovate: datasource=git-tags extractVersion=^linux-firmware-(?<version>.*)$ depName=git://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git
ARG LINUX_FIRMWARE_VERSION=20231030

# RTL 8125
WORKDIR /lib/firmware/rtl_nic
RUN wget -r -nd -e robots=no -A '*.fw' --accept-regex '/plain/' \
    https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/rtl_nic?h=${LINUX_FIRMWARE_VERSION}

# Intel UHD 770 / 13th gen Intel GPUs
WORKDIR /lib/firmware/i915
RUN wget -r -nd -e robots=no -A '*.bin' --accept-regex '/plain/' \
    https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915?h=${LINUX_FIRMWARE_VERSION}

# Update kernel modules
RUN kernel=$(ls /lib/modules | head -n1) && \
    dracut -v -f "/boot/initrd-${kernel}" "${kernel}" && \
    ln -sf "initrd-${kernel}" /boot/initrd && \
    kernel=$(ls /lib/modules | head -n1) && \
    depmod -a "${kernel}"