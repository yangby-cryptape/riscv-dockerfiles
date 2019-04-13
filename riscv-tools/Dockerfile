FROM yangby0cryptape/riscv-gnu-toolchain:rv64imc-newlib-20190414 AS builder
MAINTAINER Boyu Yang <yangby@cryptape.com>

# Install system packages
RUN set -eux; \
    apt-get update; \
    apt-get upgrade --assume-yes; \
    \
# for dependencies
    apt-get install --assume-yes --no-install-recommends \
        libusb-1.0-0-dev \
        device-tree-compiler \
        pkg-config \
# for tools
        git \
        ca-certificates \
        ;

# Fetch source code of riscv tools
RUN set -eux; \
    git clone --depth=1 --branch=master --single-branch \
    https://github.com/riscv/riscv-tools.git \
    /usr/src/riscv-tools;

# Sync submodules
RUN set -eux; \
    cd /usr/src/riscv-tools; \
    git submodule update --init --recursive --recommend-shallow;

# Compile
RUN set -eux; \
    cd /usr/src/riscv-tools; \
    export MAKEFLAGS="-j$(($(nproc)+1))"; \
# do not compile tests
    sed -i "/^build_project riscv-tests /d" build.sh; \
    ./build.sh;

FROM ubuntu:bionic
MAINTAINER Boyu Yang <yangby@cryptape.com>

COPY --from=builder "/opt/riscv" "/opt/riscv"

ENV RISCV="/opt/riscv"
ENV PATH="${PATH}:${RISCV}/bin"

# Install system packages
RUN set -eux; \
    apt-get update; \
    apt-get upgrade --assume-yes; \
    \
# for dependencies
    apt-get install --assume-yes --no-install-recommends \
    # both for riscv gnu toolchain and riscv tools
        autoconf \
        automake \
        autotools-dev \
        curl \
        libmpc-dev \
        libmpfr-dev \
        libgmp-dev \
        gawk \
        build-essential \
        bison \
        flex \
        texinfo \
        gperf \
        libtool \
        patchutils \
        bc \
        zlib1g-dev \
        libexpat-dev \
        \
    # only for riscv tools
        libusb-1.0-0-dev \
        device-tree-compiler \
        pkg-config \
        ; \
    \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*;

RUN set -eux; \
    mkdir -p /work; \
    chmod 777 /work;

WORKDIR /work
CMD ["riscv64-unknown-elf-gcc", "--version"]
