# Dockerfile - alpine
# https://github.com/openresty/docker-openresty

FROM alpine:latest

MAINTAINER Evan Wies <evan@neomantra.net>

# Install APT dependencies
# Put this before the ARGS to minimize di
RUN apk add --update \
    build-base perl make \
    readline ncurses zlib-dev


# Download and untar OpenSSL, PCRE, and OpenResty
WORKDIR /tmp

ADD https://www.openssl.org/source/openssl-1.0.2e.tar.gz /tmp/openssl-1.0.2e.tar.gz
RUN tar xzf openssl-1.0.2e.tar.gz

ADD https://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.38.tar.gz /tmp/pcre-8.38.tar.gz
RUN tar xzf pcre-8.38.tar.gz

ADD https://openresty.org/download/openresty-1.9.7.3.tar.gz /tmp/openresty-1.9.7.3.tar.gz
RUN tar xzf openresty-1.9.7.3.tar.gz


# Build OpenResty
WORKDIR /tmp/openresty-1.9.7.3
RUN ./configure -j1 --with-openssl=/tmp/openssl-1.0.2e --with-pcre=/tmp/pcre-8.38 --with-ipv6 --with-pcre-jit --with-http_gunzip_module && \
    make -j1 && \
    make -j1 install


# Cleanup
WORKDIR /tmp
RUN rm -rf openssl-1.0.2e.tar.gz openssl-1.0.2e \
           pcre-8.38.tar.gz pcre-8.38 \
           openresty-1.9.7.3.tar.gz openresty-1.9.7.3
WORKDIR /
EXPOSE 80
CMD ["/use/local/openresty/nginx/sbin/nginx"]
