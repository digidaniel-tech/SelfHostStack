# syntax=docker/dockerfile:1

FROM digidanieltech/alpine:3.21.2

ENV PHP_VERSION=8.2.27 \
    PHP_INI_DIR=/usr/local/etc/php

LABEL maintainer="Daniel Vollbro <wollbro90@gmail.com>" \
      version="${PHP_VERSION}" \
      description="Docker image for PHP version ${PHP_VERSION} based on Alpine Linux"
 
RUN apk add --no-cache libxml2-dev sqlite-dev
RUN apk add --no-cache -t build-tools git build-base autoconf libtool bison re2c pkgconfig \
    && git clone --depth 1 -b "PHP-${PHP_VERSION}" https://github.com/php/php-src.git /build \
    && cd /build \
    && ./buildconf \
    && ./configure \
        --prefix=/usr/local \
        --with-config-file-path=${PHP_INI_DIR} \
        --enable-fpm \
        --with-fpm-user=www-data \
        --with-fpm-group=www-data \
    && make -j$(nproc) \
    && make install \
    && cd / \
    && apk del build-tools \
    && rm -rf /build \
    && apk cache clean

RUN mkdir -p /usr/local/etc/php-fpm.d \
    && cp /usr/local/etc/php-fpm.conf.default /usr/local/etc/php-fpm.conf \
    && cp /usr/local/etc/php-fpm.d/www.conf.default /usr/local/etc/php-fpm.d/www.conf \
    && sed -i 's/;daemonize = yes/daemonize = no/' /usr/local/etc/php-fpm.conf

RUN adduser -D -G www-data www-data

EXPOSE 9000

CMD ["php-fpm"]
