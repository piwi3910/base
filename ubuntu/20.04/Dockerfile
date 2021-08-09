FROM ubuntu:20.04

RUN export DEBIAN_FRONTEND=noninteractive &&\
    apt-get -q update && \
    apt-get -qy upgrade && \
    apt-get -qy install \
        wget \
        curl \
        unzip \
        unrar \
        openssl \
        ca-certificates \
        && \
    apt-get -y autoremove &&\
    rm -rf /var/lib/apt/lists/*
