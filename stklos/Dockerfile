# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential file texinfo \
      libffi-dev libgc-dev libgmp-dev libpcre3-dev \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://jdebp.eu/Repository/debian/dists/stable/main/binary-amd64/ptyget_6_amd64.deb \
        ptyget_6_amd64.deb
ADD https://www.stklos.net/download/stklos-1.31.tar.gz stklos.tar.gz
RUN sha256sum ptyget_6_amd64.deb stklos.tar.gz && sha256sum -c checksum
RUN dpkg -i ptyget_6_amd64.deb
RUN mkdir stklos && tar -C stklos --strip-components 1 -xf stklos.tar.gz
WORKDIR /build/stklos
RUN ./configure \
  --without-provided-bignum --without-provided-gc --without-provided-regexp
RUN ptyrun make
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      libffi6 libgc1c2 libgmp10 libpcre3 \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
RUN ln -s stklos /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
