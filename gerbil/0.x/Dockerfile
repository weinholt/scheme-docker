# -*- mode: dockerfile; coding: utf-8 -*-

# NB: There is an official Gerbil Dockerfile at
# <https://github.com/vyzo/gerbil/blob/master/Dockerfile>

FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
        build-essential \
        libsqlite3-dev \
        libssl-dev \
        openssl \
        sqlite3 \
        zlib1g-dev \
        && rm -rf /var/lib/apt/lists/*

# Build Gambit
ADD https://www.iro.umontreal.ca/~gambit/download/gambit/v4.9/source/gambit-v4_9_3.tgz gambit-v4_9_3.tgz
RUN tar -xvf gambit-v4_9_3.tgz
WORKDIR gambit-v4_9_3
RUN ./configure --prefix=/usr/local --enable-single-host
RUN make
RUN make install

# Build Gerbil (requires Gambit)
WORKDIR /root
ADD https://github.com/vyzo/gerbil/archive/v0.15.1.tar.gz gerbil-0.15.1.tar.gz
RUN tar -xvf gerbil-0.15.1.tar.gz && mv gerbil-0.15.1 gerbil
WORKDIR gerbil/src
RUN ./build.sh

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
COPY --from=build /root/gerbil/bin/ /usr/local/bin/
COPY --from=build /root/gerbil/lib/ /usr/local/lib/
RUN ln -s gxi /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
