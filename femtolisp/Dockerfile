# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential ca-certificates git \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build/femtolisp
RUN git init \
 && git remote add origin https://github.com/JeffBezanson/femtolisp.git \
 && git fetch origin ec7601076a976f845bc05ad6bd3ed5b8cde58a97 --depth 1 \
 && git reset --hard FETCH_HEAD
RUN make
RUN cp -p flisp flisp.boot /usr/local/bin/

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      libncurses6 \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
RUN ln -s flisp /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
