# -*- mode: dockerfile; coding: utf-8 -*-
# Installing the JDK or the JRE does not seem to work in buster-slim.
FROM debian:buster AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
        build-essential \
        default-jdk-headless \
        && rm -rf /var/lib/apt/lists/*
ADD https://ftp.gnu.org/pub/gnu/kawa/kawa-3.0.tar.gz kawa-3.0.tar.gz
RUN tar -xvf kawa-3.0.tar.gz
WORKDIR kawa-3.0
RUN ./configure
RUN make
RUN make install

FROM debian:buster
RUN apt-get update && apt-get -y --no-install-recommends install \
        default-jre-headless \
        && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
COPY scheme-banner /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
