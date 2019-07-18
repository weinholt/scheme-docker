# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       ca-certificates \
       curl \
       unzip \
       xz-utils \
  && rm -rf /var/lib/apt/lists/*

ADD https://github.com/leppie/IronScheme/releases/download/1.0.110/IronScheme-1.0.110-6a2cd45.zip IronScheme.zip
RUN unzip IronScheme.zip && mv IronScheme /usr/local

FROM debian:buster
COPY --from=build /usr/local/IronScheme /usr/local/IronScheme/
RUN apt-get update && apt-get -y --no-install-recommends install \
       mono-devel \
       mono-runtime \
  && rm -rf /var/lib/apt/lists/*
COPY scheme-script /usr/local/bin/
