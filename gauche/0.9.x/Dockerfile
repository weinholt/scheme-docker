# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       gauche \
  && rm -rf /var/lib/apt/lists/*
