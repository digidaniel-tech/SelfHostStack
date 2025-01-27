# syntax=docker/dockerfile:1

FROM alpine:3.21.2

ARG VERSION=6.12.0.206
ENV MONO_VERSION=${VERSION}

LABEL maintainer="Daniel Vollbro <wollbro90@gmail.com>" \
  version="${MONO_VERSION}" \
  description="Docker image for Mono version ${MONO_VERSION} based on Alpine Linux"

# Installning dependencies and builds mono, cleans up dependencies and cache when done
RUN apk add --no-cache -t build-tools bash linux-headers git autoconf libtool automake build-base gettext cmake python3 curl \
  && git clone --depth=1 -b "mono-${MONO_VERSION}" https://github.com/mono/mono /build \
  && cd /build \
  && ./autogen.sh --prefix=/usr/local \
  && make \
  && make install \
  && cd / \
  && apk del build-tools \
  && rm -rf /build \
  && apk cache clean

# Installing dependencies required to run mono
RUN apk add --no-cache bash gcc \
  && apk cache clean

WORKDIR /
