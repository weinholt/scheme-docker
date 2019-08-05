# -*- mode: dockerfile; coding: utf-8 -*-

# XXX: This uses Jessie because newer gcc versions miscompile Mosh
FROM debian:jessie-slim AS build

ADD http://storage.osdev.info/pub/mosh/mosh-current.tar.gz /tmp/mosh-current.tar.gz
WORKDIR /tmp
#COPY mosh-current.tar.gz .
COPY onig-config /usr/local/bin

RUN apt-get update && apt-get -y --no-install-recommends install \
       autoconf \
       automake \
       bison \
       build-essential \
       ca-certificates \
       cpio \
       gauche \
       git \
       libffi-dev \
       libgmp-dev \
       libonig-dev \
       libssl-dev \
       pkg-config \
       re2c \
  && rm -rf /var/lib/apt/lists/*

RUN set -uxe; \
    tar xvaf mosh-current.tar.gz; \
    cd mosh-*; \
    ./configure --prefix=/usr/local; \
    make -j4; \
    make install

RUN git clone --depth 1 git://github.com/higepon/mosh.git
WORKDIR mosh
RUN set -uxe; \
    ./gen-git-build.sh; \
    ./configure; \
    make -j4; \
    make install

FROM debian:jessie-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
       libssl1.0.0 \
       libffi6 \
       libgmp10 \
       libonig2 \
  && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/share/ /usr/local/share/
COPY --from=build /usr/local/bin/ /usr/local/bin/
COPY scheme-banner /usr/local/bin/
RUN ln -s mosh /usr/local/bin/scheme-script
CMD ["scheme-banner"]
