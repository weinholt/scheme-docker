# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
        build-essential \
        && rm -rf /var/lib/apt/lists/*
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

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
RUN ln -s scheme48 /usr/local/bin/banner-scheme
CMD ["banner-scheme"]
