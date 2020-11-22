# -*- mode: dockerfile; coding: utf-8 -*-
FROM alpine:3.8 AS build
WORKDIR /tmp/build
COPY checksum checksum
ADD https://scheme.fail/releases/loko-0.7.0.tar.gz loko.tar.gz
RUN sha256sum loko.tar.gz && sha256sum -c checksum
RUN tar --strip-components 1 -xvzf loko.tar.gz
RUN apk add make
RUN make
RUN make install

FROM scratch
COPY --from=build /usr/local /usr/local
ENTRYPOINT ["/usr/local/bin/loko"]
