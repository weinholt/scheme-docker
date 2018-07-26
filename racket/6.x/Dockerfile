# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster
RUN apt-get update && apt-get -y --no-install-recommends install \
       racket \
  && rm -rf /var/lib/apt/lists/*

COPY scheme-script /usr/bin/
