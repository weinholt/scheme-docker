# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       build-essential \
       ca-certificates \
       cmake \
       libffi-dev \
       libgc-dev \
       libssl-dev \
       libz-dev \
       wget \
  && rm -rf /var/lib/apt/lists/*

RUN set -uxe; \
    expectedSha256="b946b168fca70f84d922bcfa2125e2e64ad5fb8cf67e4204deb43dd2dcdedb0e"; \
    url="https://bitbucket.org/ktakashi/sagittarius-scheme/downloads/sagittarius-0.9.6.tar.gz"; \
    wget --no-verbose "$url" -O ss.tgz; \
    echo "${expectedSha256} *ss.tgz" | sha256sum -c -; \
    mkdir ss; \
    tar -C ss --strip-components 1 -xaf ss.tgz; \
    rm ss.tgz
WORKDIR ss
RUN set -uxe; \
    cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/sagittarius; \
    make; \
    make install; \
    make clean

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
       libffi6 \
       libgc1c2 \
       libssl1.1 \
  && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/sagittarius/ /usr/local/sagittarius/
COPY scheme-banner scheme-script /usr/local/bin/
ENV LD_LIBRARY_PATH "/usr/local/sagittarius/lib:${LD_LIBRARY_PATH}"
ENV PATH "/usr/local/sagittarius/bin:${PATH}"
CMD scheme-banner
