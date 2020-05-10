ARG PHP_VERSION

FROM php:${PHP_VERSION}-fpm

LABEL maintainer="liumingliang <liumingliang@qie.tv>"

RUN sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security-cdn.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list

ENV DEBIAN_FRONTEND noninteractive

RUN set -xe; \
    apt-get update -yqq && \
    apt-get upgrade -yqq && \
    pecl channel-update pecl.php.net

RUN apt-get install -yqq \
        apt-utils \
        libzip-dev zip unzip \
        gcc g++ \
        make autoconf automake \
        openssl libssl-dev

###########################################################################
# 设置时区
###########################################################################

RUN ln -snf /usr/share/zoneinfo/PRC /etc/localtime && echo PRC > /etc/timezone

###########################################################################
# 基本扩展 目前仅按照首字母顺序安装 依赖性需要重新安装
###########################################################################

###########################################################################
# apcu:
###########################################################################

ARG INSTALL_APCU=false

RUN if [ ${INSTALL_APCU} = true ]; then \
    pecl install apcu && \
    docker-php-ext-enable apcu \
;fi

###########################################################################
# bcmath:
###########################################################################

ARG INSTALL_BCMATH=false

RUN if [ ${INSTALL_BCMATH} = true ]; then \
    docker-php-ext-install bcmath \
;fi

###########################################################################
# bz2:
###########################################################################

ARG INSTALL_BZ2=false

RUN if [ ${INSTALL_BZ2} = true ]; then \
    apt-get install -y libbz2-dev && \
    docker-php-ext-install bz2 \
;fi

###########################################################################
# calendar:
###########################################################################

ARG INSTALL_CALENDAR=false

RUN if [ ${INSTALL_CALENDAR} = true ]; then \
    docker-php-ext-configure calendar && \
    docker-php-ext-install calendar \
;fi

###########################################################################
# exif:
###########################################################################

ARG INSTALL_EXIF=false

RUN if [ ${INSTALL_EXIF} = true ]; then \
    docker-php-ext-install exif \
;fi

###########################################################################
# gd:
###########################################################################

ARG INSTALL_GD=false
RUN if [ ${INSTALL_GD} = true ]; then \
    apt-get install -y libpng-dev && \
    docker-php-ext-install gd \
;fi

###########################################################################
# gearman:
###########################################################################

ARG INSTALL_GEARMAN=false

RUN if [ ${INSTALL_GEARMAN} = true ]; then \
    apt-get -y install libgearman-dev git && \
    cd /tmp && \
    curl -L https://github.com/wcgallego/pecl-gearman/archive/gearman-2.0.5.zip -O && \
    unzip gearman-2.0.5.zip && \
    mv pecl-gearman-gearman-2.0.5 pecl-gearman && \
    cd /tmp/pecl-gearman && \
    phpize && \
    ./configure && \
    make -j$(nproc) && \
    make install && \
    cd / && \
    rm /tmp/gearman-2.0.5.zip && \
    rm -r /tmp/pecl-gearman && \
    docker-php-ext-enable gearman \
;fi

###########################################################################
# gettext:
###########################################################################

ARG INSTALL_GETTEXT=false

RUN if [ ${INSTALL_GETTEXT} = true ]; then \
    apt-get install -y zlib1g-dev libicu-dev g++ libpq-dev libssl-dev gettext && \
    docker-php-ext-install gettext \
;fi

###########################################################################
# grpc:
###########################################################################

ARG INSTALL_GRPC=false

RUN if [ ${INSTALL_GRPC} = true ]; then \
    pecl install grpc && \
    docker-php-ext-enable grpc \
;fi

###########################################################################
# igbinary:
###########################################################################

ARG INSTALL_IGBINARY=false

RUN if [ ${INSTALL_IGBINARY} = true ]; then \
    pecl install igbinary && \
    docker-php-ext-enable igbinary \
;fi

###########################################################################
# imap:
###########################################################################

ARG INSTALL_IMAP=false

RUN if [ ${INSTALL_IMAP} = true ]; then \
    apt-get install -y libc-client-dev libkrb5-dev && \
    docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    docker-php-ext-install imap \
;fi

###########################################################################
# memcache:
###########################################################################

ARG INSTALL_MEMCACHE=false

RUN if [ ${INSTALL_MEMCACHE} = true ]; then \
    apt-get install -y zlib1g zlib1g-dev && \
    pecl install memcache && \
    docker-php-ext-enable memcache \
;fi

###########################################################################
# memcached:
###########################################################################

ARG INSTALL_MEMCACHED=false

RUN if [ ${INSTALL_MEMCACHED} = true ]; then \
    apt-get install -y libmemcached-dev && \
    pecl install memcached-3.1.3 && \
    docker-php-ext-enable memcached \
;fi

###########################################################################
# mongodb:
###########################################################################

ARG INSTALL_MONGODB=false

RUN if [ ${INSTALL_MONGODB} = true ]; then \
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
;fi

###########################################################################
# msgpack:
###########################################################################

ARG INSTALL_MSGPACK=false

RUN if [ ${INSTALL_MSGPACK} = true ]; then \
    pecl install msgpack && \
    docker-php-ext-enable msgpack \
;fi

###########################################################################
# mysqli
###########################################################################

ARG INSTALL_MYSQLI=false

RUN if [ ${INSTALL_MYSQLI} = true ]; then \
    docker-php-ext-install mysqli \
;fi

###########################################################################
# opcache:
###########################################################################

ARG INSTALL_OPCACHE=false

RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini
RUN if [ ${INSTALL_OPCACHE} = false ]; then \
    rm -f /usr/local/etc/php/conf.d/opcache.ini \
;fi

###########################################################################
# pcntl
###########################################################################

ARG INSTALL_PCNTL=false

RUN if [ ${INSTALL_PCNTL} = true ]; then \
    docker-php-ext-install pcntl \
;fi

###########################################################################
# pcov:
###########################################################################

ARG INSTALL_PCOV=false

RUN if [ ${INSTALL_PCOV} = true ]; then \
  if [ $(php -r "echo PHP_MAJOR_VERSION;") = "7" ]; then \
    if [ $(php -r "echo PHP_MINOR_VERSION;") != "0" ]; then \
      pecl install pcov && \
      docker-php-ext-enable pcov \
    ;fi \
  ;fi \
;fi

###########################################################################
# pdo_mysql
###########################################################################

ARG INSTALL_PDO_MYSQL=false

RUN if [ ${INSTALL_PDO_MYSQL} = true ]; then \
    docker-php-ext-install pdo_mysql \
;fi

###########################################################################
# protobuf
###########################################################################

ARG INSTALL_PROTOBUF=false

RUN if [ ${INSTALL_PROTOBUF} = true ]; then \
    pecl install protobuf && \
    docker-php-ext-enable protobuf \
;fi

###########################################################################
# redis
###########################################################################

ARG INSTALL_REDIS=false

RUN if [ ${INSTALL_REDIS} = true ]; then \
      apt-get install -y libzstd-dev && \
      pecl install -o -f redis && \
      docker-php-ext-enable redis \
;fi

###########################################################################
# soap:
###########################################################################

ARG INSTALL_SOAP=false

RUN if [ ${INSTALL_SOAP} = true ]; then \
    rm /etc/apt/preferences.d/no-debian-php && \
    apt-get -y install libxml2-dev php-soap && \
    docker-php-ext-install soap \
;fi

###########################################################################
# sockets
###########################################################################

ARG INSTALL_SOCKETS=false

RUN if [ ${INSTALL_SOCKETS} = true ]; then \
    docker-php-ext-install sockets \
;fi

###########################################################################
# ssh2:
###########################################################################

ARG INSTALL_SSH2=false

RUN if [ ${INSTALL_SSH2} = true ]; then \
    apt-get -y install libssh2-1-dev && \
    pecl install -a ssh2-1.1.2 && \
    docker-php-ext-enable ssh2 \
;fi

###########################################################################
# swoole
###########################################################################

COPY ./cmake  /tmp/cmake

ARG INSTALL_SWOOLE=false

RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    cd /tmp/cmake && \
    ./configure && \
    make && \
    make install && \
    docker-php-ext-install sockets && \
    cd /tmp && \
    git clone -b v4.4.15 https://github.com/swoole/swoole-src.git && \
    cd /tmp/swoole-src && \
    phpize && \
    ./configure --enable-sockets --enable-openssl --enable-mysqlnd && \
    make && make install && \
    echo "extension=swoole.so" > /usr/local/etc/php/conf.d/docker-php-ext-swoole.ini && \
    cd /tmp && \
    git clone -b v4.4.x https://github.com/swoole/async-ext.git && \
    cd /tmp/async-ext && \
    phpize && \
    ./configure && \
    make -j 4 && make install && \
    echo "extension=swoole_async.so" >> /usr/local/etc/php/conf.d/docker-php-ext-swoole.ini \
;fi
#手动下载cmake
#COPY ./cmake  /usr/src/cmake
#
#WORKDIR /usr/src/cmake
#
#RUN ./configure && \
#    make && \
#    make install
#
#RUN apt-get install -y git && \
#    docker-php-ext-install sockets
#
#WORKDIR /usr/src
#
#RUN git clone https://github.com/swoole/swoole-src.git && \
#    cd swoole-src && \
#    git checkout v4.4.15 && \
#    phpize && \
#    ./configure --enable-sockets --enable-openssl --enable-mysqlnd && \
#    make && make install && \
#    echo "extension=swoole.so" > /usr/local/etc/php/conf.d/docker-php-ext-swoole.ini
#
#WORKDIR /usr/src
#
#RUN git clone https://github.com/swoole/async-ext.git && \
#    cd async-ext && \
#    git checkout v4.4.x && \
#    phpize && \
#    ./configure && \
#    make -j 4 && make install && \
#    echo "extension=swoole_async.so" >> /usr/local/etc/php/conf.d/docker-php-ext-swoole.ini

###########################################################################
# taint
###########################################################################

ARG INSTALL_TAINT=false

RUN if [ ${INSTALL_TAINT} = true ]; then \
    pecl install taint && \
    docker-php-ext-enable taint \
;fi

###########################################################################
# wddx:
###########################################################################

ARG INSTALL_WDDX=false

RUN if [ ${INSTALL_WDDX} = true ]; then \
    docker-php-ext-install wddx \
;fi

###########################################################################
# xdebug:
###########################################################################

ARG INSTALL_XDEBUG=false

RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    pecl install xdebug && \
    docker-php-ext-enable xdebug \
;fi

COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN if [ ${INSTALL_XDEBUG} = false ]; then \
    rm /usr/local/etc/php/conf.d/xdebug.ini \
;fi

###########################################################################
# Xhprof:
###########################################################################

ARG INSTALL_XHPROF=false

RUN if [ ${INSTALL_XHPROF} = true ]; then \
    if [ $(php -r "echo PHP_MAJOR_VERSION;") = 7 ]; then \
      curl -L -o /tmp/xhprof.tar.gz "https://github.com/tideways/php-xhprof-extension/archive/v5.0.1.tar.gz"; \
    else \
      curl -L -o /tmp/xhprof.tar.gz "https://codeload.github.com/phacility/xhprof/tar.gz/master"; \
    fi \
    && mkdir -p xhprof \
    && tar -C xhprof -zxvf /tmp/xhprof.tar.gz --strip 1 \
    && ( \
        cd xhprof \
        && phpize \
        && ./configure \
        && make \
        && make install \
    ) \
    && rm -r xhprof \
    && rm /tmp/xhprof.tar.gz \
;fi

COPY ./xhprof.ini /usr/local/etc/php/conf.d

RUN if [ ${INSTALL_XHPROF} = false ]; then \
    rm /usr/local/etc/php/conf.d/xhprof.ini \
;fi

###########################################################################
# xsl:
###########################################################################

ARG INSTALL_XSL=false

RUN if [ ${INSTALL_XSL} = true ]; then \
    apt-get -y install libxslt-dev && \
    docker-php-ext-install xsl \
;fi

###########################################################################
# yaml:
###########################################################################

USER root

ARG INSTALL_YAML=false

RUN if [ ${INSTALL_YAML} = true ]; then \
    apt-get install libyaml-dev -y ; \
    pecl install yaml && \
    docker-php-ext-enable yaml \
;fi

###########################################################################
# zip:
###########################################################################

ARG INSTALL_ZIP=false

RUN if [ ${INSTALL_ZIP} = true ]; then \
    if [ ${PHP_VERSION} = "7.3" ] || [ ${PHP_VERSION} = "7.4" ]; then \
        docker-php-ext-configure zip; \
    else \
        docker-php-ext-configure zip --with-libzip; \
    fi && \
    docker-php-ext-install zip \
;fi

###########################################################################
# faketime:
###########################################################################

ARG INSTALL_FAKETIME=false

RUN if [ ${INSTALL_FAKETIME} = true ]; then \
    apt-get install -y libfaketime && \
    echo "/usr/lib/x86_64-linux-gnu/faketime/libfaketime.so.1" > /etc/ld.so.preload \
;fi

USER root

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

ARG LOCALE=POSIX
ENV LC_ALL ${LOCALE}

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000