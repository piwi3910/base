ARG VERSION=default

FROM ubuntu:${VERSION}

RUN export DEBIAN_FRONTEND=noninteractive &&\
    apt-get -q update && \
    apt-get -qy upgrade && \
    apt-get -qy install \
        wget \
        curl \
        grep \
        unzip \
        unrar \
        openssl \
        ca-certificates \
        sed \
        jq \
        && \
    apt-get -y autoremove &&\
    rm -rf /var/lib/apt/lists/*

