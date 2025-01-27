# syntax=docker/dockerfile:1

FROM alpine:3.21.2

##################################################
# Mono install (from source)
#################################################

ARG VERSION=6.12.0.206

RUN apk update && apk add --no-cache -t build-tools bash linux-headers git autoconf libtool automake build-base gettext cmake python3 curl

RUN git clone https://github.com/mono/mono /tmp/mono
WORKDIR /tmp/mono
RUN git checkout tags/mono-${VERSION}


RUN ./autogen.sh --prefix=/usr/local \
  && make \
  && make install

#RUN apk del build-tools

# TODO: Move this to the beginning
#RUN apk add --no-cache bash gcc

##################################################
# Sonarr install
#################################################

RUN apk update && apk add --no-cache tar dos2unix
RUN mkdir -p /app/sonarr/bin

ADD https://services.sonarr.tv/v1/download/main/latest?version=4&os=linux&arch=x64 /tmp/sonarr.tar.gz

RUN tar xzf /tmp/sonarr.tar.gz -C /app/sonarr/bin --strip-components=1

WORKDIR /app/sonarr/bin
