# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://ccrma.stanford.edu/software/s7/s7.tar.gz s7.tar.gz
RUN sha256sum s7.tar.gz && sha256sum -c checksum
RUN tar -xf s7.tar.gz
WORKDIR /build/s7
RUN touch mus-config.h
RUN gcc s7.c -o s7 -DWITH_MAIN -I. -O2 -g -ldl -lm -rdynamic
RUN ./s7 libc.scm

FROM debian:buster-slim
COPY --from=build /build/s7/ /usr/local/s7/
COPY scheme-banner /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
