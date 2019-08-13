# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
        autoconf \
        build-essential \
        ca-certificates \
        git \
        && rm -rf /var/lib/apt/lists/*

# Scheme 48
ADD http://www.s48.org/1.9.2/scheme48-1.9.2.tgz scheme48-1.9.2.tgz
RUN tar -xvf scheme48-1.9.2.tgz
WORKDIR scheme48-1.9.2
RUN ./configure
RUN make
#RUN make test  # Works, but takes quite a while.
RUN make install

# scsh (requires Scheme 48)
WORKDIR /root
RUN git clone --depth=1 https://github.com/scheme/scsh.git
WORKDIR scsh
RUN git submodule update --init
RUN autoreconf
RUN ./configure
RUN make
RUN make test
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s scsh /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
