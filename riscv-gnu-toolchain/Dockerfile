FROM ubuntu:bionic AS builder
MAINTAINER Boyu Yang <yangby@cryptape.com>

ENV RISCV_ARCH="rv64imc"

# The install-prefix for executable files
ENV RISCV="/opt/riscv"
RUN set -eux; \
    mkdir -p "${RISCV}";

# Install system packages
RUN set -eux; \
    apt-get update; \
    apt-get upgrade --assume-yes; \
    \
# for dependencies
    apt-get install --assume-yes --no-install-recommends \
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
# for tools
        git \
        ca-certificates \
        ;

# Fetch source code of riscv gnu toolchain
RUN set -eux; \
    git clone --depth=1 --branch=master --single-branch \
    https://github.com/riscv/riscv-gnu-toolchain.git \
    /usr/src/riscv-gnu-toolchain;

# Sync submodules
RUN set -eux; \
    cd /usr/src/riscv-gnu-toolchain; \
    git submodule update --init --recursive --recommend-shallow;

# Compile
RUN set -eux; \
    cd /usr/src/riscv-gnu-toolchain; \
    export MAKEFLAGS="-j$(($(nproc)+1))"; \
    ./configure --prefix="${RISCV}" --with-arch="${RISCV_ARCH}"; \
    make;

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
        ; \
    \
    apt-get clean; \
    rm -rf /var/lib/apt/lists/*;

RUN set -eux; \
    mkdir -p /work; \
    chmod 777 /work;

WORKDIR /work
CMD ["riscv64-unknown-elf-gcc", "--version"]
