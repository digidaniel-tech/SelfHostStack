FROM digidanieltech/alpine:3.21.2

ARG VERSION=3.11.11
ENV PYTHON_VERSION="${VERSION}"
ENV GIT_BRANCH="v${PYTHON_VERSION}"

RUN set -eux; \
  apk add --no-cache libbz2; \
  apk add --no-cache -t .build-tools git gcc  make zlib-dev libffi-dev \
  openssl-dev musl-dev bash coreutils bzip2-dev

RUN set -eux;\
  git clone --depth 1 -b "${GIT_BRANCH}" https://github.com/python/cpython.git /build; \
  cd /build; \
  ./configure \
    --build="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
  	--enable-loadable-sqlite-extensions \
    --enable-option-checking=fatal \
	  --enable-shared \
	  --with-lto \
	  --with-ensurepip; \
  make -j$(nproc); \
  make -j$(nproc) install; \
  cd /; \
  rm -rf /build; \
  apk del .build-tools; \
  apk cache clean;

ENV PATH="/usr/local/bin:$PATH"
