# syntax=docker/dockerfile:1

FROM digidanieltech/mono:6.12.0.206-alpine3.21.2

RUN apk update && apk add --no-cache tar dos2unix
RUN mkdir -p /app/sonarr/bin

ADD https://services.sonarr.tv/v1/download/main/latest?version=4&os=linux&arch=x64 /tmp/sonarr.tar.gz

RUN tar xzf /tmp/sonarr.tar.gz -C /app/sonarr/bin --strip-components=1

WORKDIR /app/sonarr/bin
