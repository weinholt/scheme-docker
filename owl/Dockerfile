# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://gitlab.com/owl-lisp/owl/-/archive/v0.1.19/owl-v0.1.19.tar.gz owl.tar.gz
RUN sha256sum owl.tar.gz && sha256sum -c checksum
RUN mkdir owl && tar -C owl --strip-components 1 -xf owl.tar.gz
WORKDIR /build/owl
RUN make PREFIX=/usr/local
RUN make PREFIX=/usr/local install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
COPY scheme-banner /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
