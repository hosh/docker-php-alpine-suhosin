FROM php:5.6-fpm-alpine
MAINTAINER talktohosh@gmail.com

# Check latest stable version here: https://suhosin.org/stories/download.html
ENV SUHOSIN_VERSION=0.9.38

RUN \
    apk add --update --no-cache --virtual .build-deps \
        libpng-dev \
        zlib-dev \
        libjpeg-turbo-dev \
        ffmpeg-dev \
        php5-dev \
        binutils \
        cmake \
        git \
        autoconf \
        build-base \
        openssl && \

    wget https://download.suhosin.org/suhosin-$SUHOSIN_VERSION.tar.gz -P /tmp && \
    tar xvfz /tmp/suhosin-$SUHOSIN_VERSION.tar.gz -C /tmp && \
    cd /tmp/suhosin-$SUHOSIN_VERSION && \
 # Alpine linux has flock() stuff in file.h
    sed -i '1i#include <sys/file.h>' log.c && \
    phpize && \
    ./configure && \
    make && \
    cp modules/suhosin.so /usr/lib/php5/modules/ && \
    echo -e "extension=suhosin.so\nsuhosin.executor.disable_eval = On" > /etc/php5/conf.d/suhosin.ini && \

    rm /tmp/suhosin-$SUHOSIN_VERSION.tar.gz && \
    rm -rf /tmp/suhosin-$SUHOSIN_VERSION && \

    apk del .build-deps && \
