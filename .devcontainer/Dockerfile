FROM mcr.microsoft.com/devcontainers/python:3.11-noble

# Prevent interactive prompts during package installation.
ENV DEBIAN_FRONTEND=noninteractive

# Install essential dependencies.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        apt-transport-https \
        build-essential \
        ca-certificates \
        curl \
        file \
        git \
        gnupg \
        iproute2 \
        libbz2-dev \
        libcurl4-openssl-dev \
        libffi-dev \
        libgdbm-dev \
        libidn2-0 \
        liblzma-dev \
        libncurses-dev \
        libncursesw6 \
        libnss3-dev \
        libreadline-dev \
        libsqlite3-dev \
        libssl-dev \
        libtinfo6 \
        locales \
        lsb-release \
        net-tools \
        psmisc \
        socat \
        sudo \
        unzip \
        util-linux \
        wget \
        zip \
        zlib1g-dev && \
    rm -rf /var/lib/apt/lists/*

# Install Node.js 20.x (LTS)
RUN cd /tmp && \
    curl -sSL https://nodejs.org/dist/v20.18.0/node-v20.18.0-linux-x64.tar.xz -o node.tar.xz && \
    tar -xJf node.tar.xz && \
    mv node-v20.18.0-linux-x64 /usr/local/node && \
    rm node.tar.xz
ENV PATH="/usr/local/node/bin:${PATH}"

# Install google-cloud-sdk
RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    apt-get update -y && \
    apt-get install -y google-cloud-sdk && \
    rm -rf /var/lib/apt/lists/*

# Ensure locales are generated
RUN locale-gen en_US.UTF-8

# Create a non-root user
RUN if ! getent group clusterfuzz; then groupadd clusterfuzz; fi && \
    if ! getent passwd clusterfuzz; then useradd -m -g clusterfuzz -s /bin/bash clusterfuzz; fi && \
    echo "clusterfuzz ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER clusterfuzz
WORKDIR /workspaces/clusterfuzz

# Set environment variables
ENV CLOUDSDK_PYTHON=python3.11
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV PYTHONIOENCODING UTF-8
