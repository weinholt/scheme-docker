# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:stretch AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
       ca-certificates \
       curl \
       unzip \
       xz-utils \
  && rm -rf /var/lib/apt/lists/*

# RUN set -uxe; \
#     expectedSha256="ba2ad8d5e4bad30838365a3b75f0c038511dd4d489fb7b0802c6a0dc016c8525"; \
#     url="http://build.ironscheme.net/IronScheme-c2e92e8.tar.xz"; \
#     curl -L "$url" -o is.tar.xz; \
#     echo "${expectedSha256} *is.tar.xz" | sha256sum -c -; \
#     tar -C /usr/local --no-same-owner --no-same-permissions -xaf is.tar.xz; \
#     rm is.tar.xz

# The build artifact was removed from
# https://ci.appveyor.com/project/leppie/ironscheme/build/1.0.102 and
# the builds on build.ironscheme.net are old. Just send an email if
# you want a clone of the repo this was built from; it is identical to
# the file that was on appveyor.
ADD IronScheme-1.0.102-de0d45c.zip .
RUN unzip IronScheme-1.0.102-de0d45c.zip && mv IronScheme /usr/local

FROM debian:stretch
COPY --from=build /usr/local/IronScheme /usr/local/IronScheme/
RUN apt-get update && apt-get -y --no-install-recommends install \
       mono-reference-assemblies-4.0 \
       mono-runtime \
  && rm -rf /var/lib/apt/lists/*
COPY scheme-script /usr/local/bin/
