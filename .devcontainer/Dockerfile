FROM ubuntu:24.04

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install minimal build dependencies for Python 3.11
RUN apt-get update && apt-get install -y \
    build-essential \
    ca-certificates \
    curl \
    git \
    libbz2-dev \
    libffi-dev \
    liblzma-dev \
    libncursesw5-dev \
    libreadline-dev \
    libsqlite3-dev \
    libssl-dev \
    wget \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

# Create a working directory
WORKDIR /workspace

# Copy the Python installation script from the repository
COPY local/install_python_from_source.bash /tmp/

# Run the installation script. It installs Python 3.11.9 to /root/.localpython
RUN bash /tmp/install_python_from_source.bash

# Update PATH to include the new Python installation
ENV PATH="/root/.localpython/bin:${PATH}"

# Set Python 3.11 as the default python3
RUN ln -s /root/.localpython/bin/python3.11 /usr/local/bin/python3 && \
    ln -s /root/.localpython/bin/python3.11 /usr/local/bin/python

# Verify the installation
RUN python3 --version && pip3 --version
