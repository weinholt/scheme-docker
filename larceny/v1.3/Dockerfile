# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       ca-certificates \
       wget \
  && rm -rf /var/lib/apt/lists/*

RUN set -uxe; \
    expectedSha256="f04a63e69d6aa2dfd633a5583628d41ca545e7327ce801ba3c61bf16b7184821"; \
    url="http://www.larcenists.org/LarcenyReleases/larceny-1.3-bin-native-ia32-linux86.tar.gz"; \
    wget --no-verbose "$url" -O ls.tgz; \
    echo "${expectedSha256} *ls.tgz" | sha256sum -c -; \
    mkdir -p /usr/local/lib/larceny; \
    tar -C /usr/local/lib/larceny --strip-components 1 -xzf ls.tgz; \
    rm ls.tgz

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
       libc6-i386 \
  && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/lib/larceny/ /usr/local/lib/larceny/
COPY scheme-banner scheme-script /usr/local/bin/
RUN ln -s ../lib/larceny/larceny /usr/local/bin/larceny
CMD ["scheme-banner"]
