# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
        build-essential autoconf automake libtool m4 wget \
        libgmp-dev libpcre3-dev libssl-dev libunistring-dev \
        && rm -rf /var/lib/apt/lists/*
WORKDIR /build
RUN wget --no-verbose ftp://ftp-sop.inria.fr/indes/fp/Bigloo/bigloo4.3f.tar.gz
RUN set -uxe; \
    expectedSha256="09facb8440ff06edae30b2184a589836f5c598585ef096ca8471e1903e929027"; \
    echo "${expectedSha256} *bigloo4.3f.tar.gz" | sha256sum -c -
RUN tar -xf bigloo4.3f.tar.gz && mv bigloo4.3f bigloo
WORKDIR bigloo
RUN ./configure
RUN make
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s bigloo /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
