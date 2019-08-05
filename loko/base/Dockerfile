# -*- mode: dockerfile; coding: utf-8 -*-
FROM akkuscm/akku:latest AS build
WORKDIR /tmp/build
ADD https://gitlab.com/weinholt/loko/-/archive/v0.1.0/loko-v0.1.0.tar.gz loko.tar.gz
RUN set -uxe; \
    expectedSha256="f507c12c72991cbae110d5a824d1424205aa424c9862348084e5ce0366895db8"; \
    echo "${expectedSha256} *loko.tar.gz" | sha256sum -c -
RUN tar --strip-components 1 -xvzf loko.tar.gz
RUN akku --debug install
RUN ./build.sh

FROM scratch
COPY --from=build /tmp/build/loko /bin/
ENTRYPOINT ["/bin/loko"]
