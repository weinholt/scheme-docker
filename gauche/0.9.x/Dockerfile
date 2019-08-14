# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential \
        libtool pkg-config curl ca-certificates unzip netbase \
        openssh-client zlib1g-dev \
    && mkdir /home/gauche \
    && cd /home/gauche \
    && curl https://raw.githubusercontent.com/shirok/get-gauche/master/get-gauche.sh > get-gauche.sh \
    && chmod +x get-gauche.sh \
    && ./get-gauche.sh --prefix /usr/local --version latest --auto \
    && apt-get remove -y --purge --auto-remove build-essential \
        libtool pkg-config curl unzip \
    && rm -rf /var/lib/apt/lists/*

FROM debian:buster-slim
COPY --from=build /usr/local/ /usr/local/
COPY scheme-banner /usr/local/bin/
RUN ln -s gosh /usr/bin/scheme-r7rs
CMD ["scheme-banner"]
