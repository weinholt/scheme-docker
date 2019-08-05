# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       build-essential \
       ca-certificates \
       cpio \
       git \
       libffi-dev \
       libgmp-dev \
  && rm -rf /var/lib/apt/lists/*

RUN git clone --depth 1 https://github.com/weinholt/ypsilon.git
WORKDIR ypsilon
RUN set -uxe; \
    make; \
    make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
       libffi6 \
       libgmp10 \
  && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/share/ /usr/local/share/
COPY --from=build /usr/local/bin/ /usr/local/bin/
COPY scheme-script /usr/local/bin
RUN ln -s ypsilon /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
