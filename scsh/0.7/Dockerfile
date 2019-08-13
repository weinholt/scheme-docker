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
RUN set -uxe; \
    expectedSha256="9c4921a90e95daee067cd2e9cc0ffe09e118f4da01c0c0198e577c4f47759df4"; \
    echo "${expectedSha256} *scheme48-1.9.2.tgz" | sha256sum -c -
RUN tar -xvf scheme48-1.9.2.tgz
WORKDIR scheme48-1.9.2
RUN ./configure
RUN make
#RUN make test  # Works, but takes quite a while.
RUN make install

# scsh (requires Scheme 48)
WORKDIR /root
RUN (git clone --depth=1 https://github.com/scheme/scsh.git && cd scsh && git reset --hard 114432435e4eadd54334df6b37fcae505079b49f)
WORKDIR scsh
RUN (git submodule update --init && cd rx && git reset --hard d3231ad13de2b44e3ee173b1c9d09ff165e8b6d5)
RUN autoreconf
RUN ./configure
RUN make
RUN make test
RUN make install

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s scsh /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
