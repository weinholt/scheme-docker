# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      autoconf build-essential ca-certificates git \
 && rm -rf /var/lib/apt/lists/*

# Scheme 48
WORKDIR /build
COPY checksum checksum
ADD http://www.s48.org/1.9.2/scheme48-1.9.2.tgz scheme48.tgz
RUN sha256sum scheme48.tgz && sha256sum -c checksum
RUN mkdir scheme48 && tar -C scheme48 --strip-components 1 -xf scheme48.tgz
WORKDIR /build/scheme48
RUN ./configure
RUN make
#RUN make test  # Works, but takes quite a while.
RUN make install

# scsh (requires Scheme 48)
WORKDIR /build/scsh
RUN git init \
 && git remote add origin https://github.com/scheme/scsh.git \
 && git fetch origin 114432435e4eadd54334df6b37fcae505079b49f --depth 1 --tags \
 && git reset --hard FETCH_HEAD
RUN git submodule update --init \
 && (cd rx && git reset --hard d3231ad13de2b44e3ee173b1c9d09ff165e8b6d5)
RUN autoreconf
RUN ./configure
RUN make
RUN make test
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s scsh /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
