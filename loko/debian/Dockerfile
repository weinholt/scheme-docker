# -*- mode: dockerfile; coding: utf-8 -*-
FROM akkuscm/akku:latest AS build
WORKDIR /tmp/build
COPY checksum checksum
ADD https://scheme.fail/releases/loko-0.2.1.tar.gz loko.tar.gz
RUN sha256sum loko.tar.gz && sha256sum -c checksum
RUN tar --strip-components 1 -xvzf loko.tar.gz
RUN akku --debug install
RUN apk add make
RUN make

FROM debian:buster-slim
COPY --from=build /tmp/build/loko /usr/local/bin/
RUN ln -s loko /usr/local/bin/scheme-banner
RUN ln -s loko /usr/local/bin/scheme-script
CMD ["scheme-banner"]
