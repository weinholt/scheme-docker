# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       gauche \
  && rm -rf /var/lib/apt/lists/*

RUN ln -s gosh /usr/bin/scheme-r7rs
