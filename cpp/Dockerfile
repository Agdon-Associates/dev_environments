# Base image

ARG VARIANT="focal"
FROM mcr.microsoft.com/vscode/devcontainers/cpp:0-$(VARIANT)

# Development tools

RUN export DEBIAN_FRONTEND=noninteractive && apt-get update \
    && apt-get -y install \
    silversearcher-ag \
    expect \
    doxygen \
    graphviz \
    colordiff \
    && apt-get -y autoremove \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/*

    