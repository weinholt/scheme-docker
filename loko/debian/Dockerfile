# -*- mode: dockerfile; coding: utf-8 -*-
FROM akkuscm/akku:latest AS build
WORKDIR /tmp/build
COPY checksum checksum
ADD https://scheme.fail/releases/loko-0.4.1.tar.gz loko.tar.gz
RUN sha256sum loko.tar.gz && sha256sum -c checksum
RUN tar --strip-components 1 -xvzf loko.tar.gz
RUN akku --debug install
RUN apk add make
RUN make
RUN make install && rm /usr/local/bin/scheme-banner

FROM debian:buster-slim
COPY --from=build /usr/local /usr/local
RUN ln -s loko /usr/local/bin/scheme-banner && ln -s loko /usr/local/bin/scheme-script
CMD ["scheme-banner"]
