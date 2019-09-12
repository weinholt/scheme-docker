# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential autoconf automake libtool m4 curl \
      libgmp-dev libpcre3-dev libssl-dev libunistring-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
RUN curl --fail --silent --show-error -o bigloo.tar.gz \
      ftp://ftp-sop.inria.fr/indes/fp/Bigloo/bigloo4.3f.tar.gz
RUN sha256sum bigloo.tar.gz && sha256sum -c checksum
RUN mkdir bigloo && tar -C bigloo --strip-components 1 -xf bigloo.tar.gz
WORKDIR bigloo
RUN ./configure
RUN make
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s bigloo /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
