# -*- mode: dockerfile; coding: utf-8 -*-
FROM akkuscm/akku:latest AS build
WORKDIR /tmp/build
ADD https://scheme.fail/releases/loko-0.2.0.tar.gz loko.tar.gz
RUN set -uxe; \
    expectedSha256="f641de424bfecaedcde90081ae7b944bf050ae4bcbcf652194e8894753c3c508"; \
    echo "${expectedSha256} *loko.tar.gz" | sha256sum -c -
RUN tar --strip-components 1 -xvzf loko.tar.gz
RUN akku --debug install
RUN apk add make
RUN make

FROM debian:buster-slim
COPY --from=build /tmp/build/loko /usr/local/bin/
RUN ln -s loko /usr/local/bin/scheme-banner
RUN ln -s loko /usr/local/bin/scheme-script
CMD ["scheme-banner"]
