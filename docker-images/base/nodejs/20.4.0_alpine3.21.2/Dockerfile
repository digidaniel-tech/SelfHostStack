FROM digidanieltech/python:3.11.11_alpine3.21.2

ARG VERSION="20.4.0"
ENV NODEJS_VERSION="${VERSION}"
ENV GIT_BRANCH="v${NODEJS_VERSION}"

RUN set -eux; \
  apk add --no-cache -t .build-tools git g++ gcc make build-base libbz2

RUN git clone --depth 1 -b "${GIT_BRANCH}" https://github.com/nodejs/node.git /build; \
  cd /build; \
  export CXX=g++; \
  ./configure; \
  make -j$(nproc);

