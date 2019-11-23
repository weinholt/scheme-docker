# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      unzip \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
COPY checksum checksum
ADD https://github.com/leppie/IronScheme/releases/download/1.0.212/IronScheme-1.0.212-dfc3528.zip IronScheme.zip
RUN sha256sum IronScheme.zip && sha256sum -c checksum
RUN unzip IronScheme.zip && mv IronScheme /usr/local

FROM mcr.microsoft.com/dotnet/core/runtime:3.0
COPY --from=build /usr/local/IronScheme /usr/local/IronScheme/
ENV LANG C.UTF-8
COPY ironscheme /usr/local/bin/
RUN ln -s IronScheme.ConsoleCore.dll /usr/local/IronScheme/ironscheme && \
    ln -s ironscheme /usr/local/bin/isc && \
    ln -s ironscheme /usr/local/bin/scheme-banner && \
    ln -s ironscheme /usr/local/bin/scheme-script
CMD ["scheme-banner"]
