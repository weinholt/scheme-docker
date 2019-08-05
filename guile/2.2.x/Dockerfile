# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       guile-2.2 \
  && rm -rf /var/lib/apt/lists/*

COPY scheme-script /usr/local/bin/
