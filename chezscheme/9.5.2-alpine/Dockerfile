# -*- mode: dockerfile; coding: utf-8 -*-
FROM alpine:3.8 AS build
RUN apk add --no-cache \
       ca-certificates \
       curl \
       gcc \
       libx11-dev \
       util-linux-dev \
       make \
       ncurses-dev \
       musl-dev

RUN set -uxe; \
    expectedSha256="7189f839f63e7f0e58277138f5873f91c23eae0706a5bfd7081dab5289268f10"; \
    url="https://github.com/cisco/ChezScheme/releases/download/v9.5.2/csv9.5.2.tar.gz"; \
    curl --fail --silent --show-error -L "$url" -o cs.tgz; \
    echo "${expectedSha256} *cs.tgz" | sha256sum -c -; \
    mkdir cs; \
    tar -C cs --strip-components 1 --no-same-owner --no-same-permissions -xzf cs.tgz; \
    rm cs.tgz
WORKDIR cs
RUN set -uxe; \
    sed -ie 's,expeditor,,' s/*.def; \
    sed -ie 's,#include <xlocale.h>,,' c/expeditor.c; \
    ./configure --workarea=build --threads; \
    make -C build; \
    make -C build clean
RUN set -uxe; \
    sed -ie 's,-lncurses,,' c/Mf-*; \
    rm build/boot/*/equates.h; \
    cp -fr build/boot/* boot/; \
    make clean; \
    rm -rf build; \
    ./configure --workarea=build --threads; \
    make -C build; \
    make install; \
    rm -rf /usr/lib/csv9.5.2/examples; \
    scheme --version

FROM alpine:3.8
RUN apk add --no-cache \
       libuuid
COPY --from=build /usr/lib/csv9.5.2/ /usr/lib/csv9.5.2/
COPY --from=build /usr/bin/scheme /usr/bin/
COPY scheme-banner /usr/local/bin/
RUN ln -s scheme /usr/bin/scheme-script && \
    ln -s scheme /usr/bin/petite
CMD ["scheme-banner"]
