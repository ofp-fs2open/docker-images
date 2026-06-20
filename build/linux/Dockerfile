FROM --platform=$TARGETPLATFORM docker.io/library/ubuntu:22.04

ARG TARGETARCH

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

# install general requirements
RUN apt-get update && apt-get install -yqq \
        software-properties-common \
        build-essential \
        ca-certificates \
        gpg \
        wget \
        bash \
        sudo \
        git \
        curl \
        jq \
        zstd \
        unzip \
        file \
        fuse3 \
        gcc-9 \
        g++-9 \
        python3.10 \
        python3-pip \
        ninja-build \
        pkg-config \
        libfontconfig1 \
        libgl1-mesa-dev \
        libreadline6-dev \
        libudev-dev \
        libwayland-dev \
        libxcb-glx0-dev \
        libxcb-xinput0 \
        libxcb-xinerama0 \
        libxcb-cursor-dev \
        libxkbcommon0 \
        libxrandr-dev

# install newer cmake version into /usr/local
COPY cmake-*${TARGETARCH}.tar.gz /tmp
RUN tar xzf /tmp/cmake-*${TARGETARCH}.tar.gz --strip-components=1 -C /usr/local

# add extra compilers and tools
COPY llvm.sh /tmp/llvm.sh
RUN apt-add-repository -y ppa:ubuntu-toolchain-r/test \
    && /tmp/llvm.sh 16 \
    && apt-get update -yqq \
    && apt-get install -yqq \
        g++-13 \
        clang-tidy-16 \
        libc++-16-dev \
        libc++abi-16-dev \
        valgrind

# install local ccache
COPY ./ccache-4.8.2-linux-${TARGETARCH}/ /tmp/ccache/
RUN cd /tmp/ccache && make install

# clear apt cache and temporary files
RUN apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/*


ENTRYPOINT [ "/bin/bash" ]
