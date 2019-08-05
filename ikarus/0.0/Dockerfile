# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       build-essential \
       bzr \
       ca-certificates \
       libffi-dev \
       libgmp-dev \
  && rm -rf /var/lib/apt/lists/*

RUN bzr checkout --lightweight lp:ikarus
WORKDIR ikarus
RUN set -uxe; \
    ./configure; \
    make; \
    make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
       libffi6 \
       libgmp10 \
  && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/lib/ikarus/ /usr/local/lib/ikarus/
COPY --from=build /usr/local/bin/ /usr/local/bin/
RUN ln -s ikarus /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
