# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
        build-essential \
        && rm -rf /var/lib/apt/lists/*
WORKDIR /root
ADD https://github.com/leftmike/foment/archive/v0.4.1.tar.gz foment-0.4.1.tar.gz
RUN tar -xvf foment-0.4.1.tar.gz && mv foment-0.4.1 foment
WORKDIR foment/unix
RUN make

FROM debian:buster-slim
COPY --from=build /root/foment/unix/release/foment /usr/local/bin/
RUN ln -s foment /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
