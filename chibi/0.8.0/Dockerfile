# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:stretch AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       build-essential \
       wget \
  && rm -rf /var/lib/apt/lists/*

RUN wget http://synthcode.com/scheme/chibi/chibi-scheme-0.8.0.tgz
RUN tar -xf chibi-scheme-0.8.0.tgz
WORKDIR chibi-scheme-0.8.0
RUN set -uxe; \
    make; \
    make install

FROM debian:stretch
COPY --from=build /usr/local/ /usr/local/
COPY scheme-r7rs /usr/local/bin/
RUN ldconfig
