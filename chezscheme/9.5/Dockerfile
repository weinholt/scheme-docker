# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:stretch AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       ca-certificates \
       curl \
       gcc \
       libc6-dev \
       libx11-dev \
       make \
       ncurses-dev \
  && rm -rf /var/lib/apt/lists/*

RUN set -uxe; \
    expectedSha256="90bddd47bae6bc97d0daacbace9606a955fd32b98ff56c38bbad4d6903e182e5"; \
    url="https://github.com/cisco/ChezScheme/releases/download/v9.5/csv9.5.tar.gz"; \
    curl -L "$url" -o cs.tgz; \
    echo "${expectedSha256} *cs.tgz" | sha256sum -c -; \
    mkdir cs; \
    tar -C cs --strip-components 1 --no-same-owner --no-same-permissions -xzf cs.tgz; \
    rm cs.tgz
WORKDIR cs
RUN set -uxe; \
    sed -ie 's,expeditor,,' s/*.def; \
    sed -ie 's,#include <xlocale.h>,,' c/expeditor.c; \
    ./configure --workarea=build; \
    make -C build; \
    make -C build clean
RUN set -uxe; \
    sed -ie 's,-lncurses,,' c/Mf-*; \
    rm build/boot/*/equates.h; \
    cp -fr build/boot/* boot/; \
    make clean; \
    rm -rf build; \
    ./configure --workarea=build; \
    make -C build; \
    make install; \
    rm -rf /usr/lib/csv9.5/examples; \
    scheme --version

FROM debian:stretch
COPY --from=build /usr/bin/scheme /usr/bin/
RUN ln -s /usr/bin/scheme /usr/bin/scheme-script && ln -s /usr/bin/scheme /usr/bin/petite
COPY --from=build /usr/lib/csv9.5/ /usr/lib/csv9.5/
ENTRYPOINT ["/usr/bin/scheme"]
