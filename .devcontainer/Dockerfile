FROM ubuntu:24.04

# Prevent interactive prompts during package installation.
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

# Install essential and project dependencies.
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
        apt-transport-https \
        blackbox \
        build-essential \
        curl \
        file \
        gdb \
        git \
        iproute2 \
        libbz2-dev \
        libcurl4-openssl-dev \
        libffi-dev \
        libgdbm-dev \
        libidn12 \
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
        openjdk-17-jdk \
        psmisc \
        socat \
        software-properties-common \
        sudo \
        unzip \
        util-linux \
        wget \
        xvfb \
        zip \
        zlib1g-dev && \
    apt-get autoremove -y && \
    apt-get clean

# Build and install Python 3.11.9 from source (to match .tool-versions).
RUN curl -sS https://www.python.org/ftp/python/3.11.9/Python-3.11.9.tgz | tar -C /tmp -xzv && \
    cd /tmp/Python-3.11.9 && \
    ./configure --enable-optimizations --enable-loadable-sqlite-extensions && \
    make altinstall && \
    rm -rf /tmp/Python-3.11.9

# Set up symbolic links for python3 and pip.
RUN ln -sf /usr/local/bin/python3.11 /usr/bin/python3.11 && \
    ln -sf /usr/bin/python3.11 /usr/bin/python3 && \
    ln -sf /usr/local/bin/pip3.11 /usr/bin/pip3.11 && \
    ln -sf /usr/bin/pip3.11 /usr/bin/pip

# Install pipenv.
RUN pip install --no-cache-dir pipenv==2022.8.5

# Install Google Cloud CLI.
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | \
    tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | \
    gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg && \
    apt-get update -y && \
    apt-get install -y \
        google-cloud-cli \
        google-cloud-cli-app-engine-python \
        google-cloud-cli-app-engine-python-extras \
        google-cloud-cli-datastore-emulator \
        google-cloud-cli-pubsub-emulator

# Install patchelf 0.9 from source.
RUN curl -sS https://releases.nixos.org/patchelf/patchelf-0.9/patchelf-0.9.tar.bz2 | tar -C /tmp -xj && \
    cd /tmp/patchelf-*/ && \
    ./configure --prefix=/usr && \
    make install && \
    rm -rf /tmp/patchelf-*

# Install Node.js 19.9.0 from binary.
RUN cd /tmp && \
    curl -sSL https://nodejs.org/dist/v19.9.0/node-v19.9.0-linux-x64.tar.xz -o node.tar.xz && \
    tar -xJf node.tar.xz && \
    mv node-v19.9.0-linux-x64 /usr/local/node && \
    rm node.tar.xz
ENV PATH="/usr/local/node/bin:${PATH}"

# Set up the clusterfuzz user.
ARG USERNAME=clusterfuzz
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set up locale.
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# Common environment variables.
ENV CLOUDSDK_PYTHON=python3.11
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV PYTHONPATH=/workspaces/clusterfuzz/src

USER $USERNAME
