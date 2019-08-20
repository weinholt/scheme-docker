# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential wget \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
RUN wget --no-verbose ftp://ccrma-ftp.stanford.edu/pub/Lisp/s7.tar.gz
RUN sha256sum s7.tar.gz && sha256sum -c checksum
RUN tar -xf s7.tar.gz
WORKDIR /build/s7
RUN touch mus-config.h
RUN gcc s7.c -o repl -DWITH_MAIN -I. -O2 -g -ldl -lm -Wl,-export-dynamic

# Initial run to generate libc_s7.c and compile libc_s7.so using gcc.
RUN ./repl </dev/null

FROM debian:buster-slim
COPY --from=build /build/checksum /usr/local/s7/
COPY --from=build /build/s7/ /usr/local/s7/
COPY scheme-banner /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
