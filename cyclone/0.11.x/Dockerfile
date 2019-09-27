# Need to use Debian sid because libck is not available in buster.
FROM debian:sid-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential \
      libck-dev \
      libtommath-dev \
 && rm -rf /var/lib/apt/lists/*

ADD https://github.com/justinethier/cyclone-bootstrap/archive/v0.11.3.tar.gz cyclone-bootstrap-0.11.3.tar.gz
RUN tar -xvf cyclone-bootstrap-0.11.3.tar.gz
WORKDIR cyclone-bootstrap-0.11.3
RUN make
RUN make install
RUN make test

FROM debian:sid-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      gcc libc-dev libck0 libck-dev libtommath1 libtommath-dev \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
RUN ln -s icyc /usr/local/bin/scheme-banner
CMD ["scheme-banner"]
