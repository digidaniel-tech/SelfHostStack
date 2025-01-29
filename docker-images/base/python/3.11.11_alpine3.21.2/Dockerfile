FROM digidanieltech/alpine:3.21.2 AS build

ARG VERSION=3.11.11
ENV PYTHON_VERSION="${VERSION}"
ENV GIT_BRANCH="v${PYTHON_VERSION}"

RUN set -eux; \
  apk add --no-cache -t .build-tools git gcc make zlib-dev libffi-dev \
  openssl-dev musl-dev bash coreutils bzip2-dev upx

RUN set -eux;\
  git clone --depth 1 -b "${GIT_BRANCH}" https://github.com/python/cpython.git /build; \
  cd /build; \
  ./configure \
    --build="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
    --disable-test-modules \
  	--enable-loadable-sqlite-extensions \
    --enable-option-checking=fatal \
	  --enable-shared \
	  --with-lto \
	  --with-ensurepip; \
  make -j$(nproc) CFLAGS=-Os LDFLAGS=-s; \
  make -j$(nproc) install; \
  find /usr/local -depth \
  \( -type d -a \( -name test -o -name tests \) \) -exec rm -rf '{}' +; \
  find /usr/local -name '*.pyc' -exec rm -f '{}' +; \
  find /usr/local -name '*.pyo' -exec rm -f '{}' +; \
  strip --strip-all /usr/local/bin/python3.11; \
  strip --strip-debug /usr/local/lib/libpython*.so; \
  upx --best /usr/local/bin/python3.11; \
  cd /; \
  rm -rf /build; \
  apk del .build-tools;

FROM digidanieltech/alpine:3.21.2 AS final

RUN apk add --no-cache libbz2

COPY --from=build /usr/local /usr/local

ENV PATH="/usr/local/bin:$PATH"
