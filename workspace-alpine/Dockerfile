ARG PHP_VERSION

FROM php:${PHP_VERSION}-cli-alpine

LABEL maintainer="liumingliang <liumingliang@qie.tv>"

ARG PHP_VERSION

ENV DEBIAN_FRONTEND noninteractive

###########################################################################
# 以下内容随意更改.....
###########################################################################

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories && \
    apk update && \
    apk upgrade && \
    apk add --no-cache $PHPIZE_DEPS bash gcc g++ make autoconf automake m4 \
    libzip-dev zip unzip build-base libtool file openssl curl vim && \
    apk add -U tzdata && \
    pecl channel-update pecl.php.net && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

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
    apk add --no-cache freetype freetype-dev libjpeg-turbo libjpeg-turbo-dev libpng libpng-dev gd-dev && \
    docker-php-ext-install gd \
;fi

###########################################################################
# gettext:
###########################################################################

ARG INSTALL_GETTEXT=false

RUN if [ ${INSTALL_GETTEXT} = true ]; then \
    apk add --no-cache gettext-dev && \
    docker-php-ext-install gettext \
;fi

###########################################################################
# grpc:
###########################################################################

ARG INSTALL_GRPC=false

RUN if [ ${INSTALL_GRPC} = true ]; then \
    apk add --no-cache linux-headers libressl-dev musl-dev libffi-dev libstdc++ && \
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
# intl:
###########################################################################

ARG INSTALL_INTL=false

RUN if [ ${INSTALL_INTL} = true ]; then \
    apk add --no-cache zlib-dev icu icu-dev && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl \
;fi

###########################################################################
# libsodium:
###########################################################################

ARG INSTALL_LIBSODIUM=false

RUN if [ ${INSTALL_LIBSODIUM} = true ]; then \
    apk add --no-cache libsodium libsodium-dev && \
    pecl install libsodium && \
    docker-php-ext-enable sodium \
;fi

###########################################################################
# memcache:
###########################################################################

ARG INSTALL_MEMCACHE=false

RUN if [ ${INSTALL_MEMCACHE} = true ]; then \
    pecl install memcache && \
    docker-php-ext-enable memcache \
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
# memcached:
###########################################################################

ARG INSTALL_MEMCACHED=false

RUN if [ ${INSTALL_MEMCACHED} = true ]; then \
    apk add --no-cache --update zlib zlib-dev libmemcached-libs libmemcached-dev cyrus-sasl-dev && \
    pecl install memcached-3.1.3 && \
    docker-php-ext-enable memcached \
;fi

###########################################################################
# mongodb:
###########################################################################

ARG INSTALL_MONGODB=false

RUN if [ ${INSTALL_MONGODB} = true ]; then \
    apk add --no-cache openssl-dev && \
    pecl install mongodb && \
    docker-php-ext-enable mongodb \
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
      apk add --no-cache zstd zstd-dev && \
      pecl install -o -f redis && \
      docker-php-ext-enable redis \
;fi

###########################################################################
# soap:
###########################################################################

ARG INSTALL_SOAP=false

RUN if [ ${INSTALL_SOAP} = true ]; then \
    apk add --no-cache libxml2-dev php-soap && \
    docker-php-ext-install soap \
;fi

###########################################################################
# ssh2:
###########################################################################

ARG INSTALL_SSH2=false

RUN if [ ${INSTALL_SSH2} = true ]; then \
    apk add --no-cache libssh2 libssh2-dev && \
    pecl install -a ssh2-1.1.2 && \
    docker-php-ext-enable ssh2 \
;fi

###########################################################################
# zip:
###########################################################################

RUN docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-install zip

###########################################################################
# uuid:
###########################################################################

ARG INSTALL_UUID=false

RUN if [ ${INSTALL_UUID} = true ]; then \
    apk add libuuid util-linux-dev && \
    pecl install uuid && \
    docker-php-ext-enable uuid \
;fi

###########################################################################
# xdebug:
###########################################################################

RUN pecl install xdebug

###########################################################################
# Tools
###########################################################################

###########################################################################
# Composer
###########################################################################

RUN curl -sS https://getcomposer.org/installer | php --  --filename=composer && \
    mv composer /usr/local/bin && \
    /usr/local/bin/composer config -g repo.packagist composer https://packagist.phpcomposer.com

###########################################################################
# Git
###########################################################################

RUN apk add --no-cache git openssh && \
    git config --global user.name "刘明亮" && \
    git config --global user.email "liumingliang@qie.tv"

###########################################################################
# Aliases
###########################################################################

COPY ./aliases.sh /root/aliases.sh

RUN sed -i 's/\r//' /root/aliases.sh && \
    echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source ~/aliases.sh" >> ~/.bashrc && \
	echo "" >> ~/.bashrc

WORKDIR /var/www
