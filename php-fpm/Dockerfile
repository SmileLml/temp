ARG PHP_VERSION

FROM php:7.2.33-fpm

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
        libzip-dev zip unzip

###########################################################################
# 设置时区
###########################################################################

RUN ln -snf /usr/share/zoneinfo/PRC /etc/localtime && echo PRC > /etc/timezone

###########################################################################
# 基本扩展
###########################################################################

###########################################################################
# zip:
###########################################################################

ARG INSTALL_ZIP=true

RUN if [ ${INSTALL_ZIP} = true ]; then \
    docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-install zip \
;fi

###########################################################################
# bz2:
###########################################################################

ARG INSTALL_BZ2=true

RUN if [ ${INSTALL_BZ2} = true ]; then \
    apt-get install -y libbz2-dev && \
    docker-php-ext-install bz2 \
;fi

###########################################################################
# bcmath:
###########################################################################

ARG INSTALL_BCMATH=false

RUN if [ ${INSTALL_BCMATH} = true ]; then \
    docker-php-ext-install bcmath \
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
# igbinary:
###########################################################################

ARG INSTALL_IGBINARY=false

RUN if [ ${INSTALL_IGBINARY} = true ]; then \
    pecl install http://pecl.php.net/get/igbinary-3.1.2.tgz && \
    docker-php-ext-enable igbinary \
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
# msgpack:
###########################################################################

ARG INSTALL_MSGPACK=false

RUN if [ ${INSTALL_MSGPACK} = true ]; then \
    pecl install http://pecl.php.net/get/msgpack-2.1.1.tgz && \
    docker-php-ext-enable msgpack \
;fi

###########################################################################
# protobuf:
###########################################################################

ARG INSTALL_PROTOBUF=false

RUN if [ ${INSTALL_PROTOBUF} = true ]; then \
    pecl install http://pecl.php.net/get/protobuf-3.12.4.tgz && \
    docker-php-ext-enable protobuf \
;fi

###########################################################################
# wddx:
###########################################################################

ARG INSTALL_WDDX=false

RUN if [ ${INSTALL_WDDX} = true ]; then \
    apt-get -y install libxml2-dev && \
    docker-php-ext-install wddx \
;fi

###########################################################################
# snmp:
###########################################################################

ARG INSTALL_SNMP=false

RUN if [ ${INSTALL_SNMP} = true ]; then \
    apt-get install -y libsnmp-base libsnmp-dev libsnmp-perl && \
    docker-php-ext-install gettext \
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
# apcu:
###########################################################################

ARG INSTALL_APCU=false

RUN if [ ${INSTALL_APCU} = true ]; then \
    pecl install http://pecl.php.net/get/apcu-5.1.18.tgz && \
    docker-php-ext-enable apcu \
;fi

###########################################################################
# gd:
###########################################################################

ARG INSTALL_GD=false

RUN if [ ${INSTALL_GD} = true ]; then \
    apt-get install -y libgd-dev  && \
    docker-php-ext-install gd \
;fi

###########################################################################
# grpc:
###########################################################################

ARG INSTALL_GRPC=false

RUN if [ ${INSTALL_GRPC} = true ]; then \
    pecl install http://pecl.php.net/get/grpc-1.30.0.tgz && \
    docker-php-ext-enable grpc \
;fi

###########################################################################
# memcache:
###########################################################################

ARG INSTALL_MEMCACHE=false

RUN if [ ${INSTALL_MEMCACHE} = true ]; then \
    apt-get install -y zlib1g zlib1g-dev && \
    pecl install http://pecl.php.net/get/memcache-4.0.5.2.tgz && \
    docker-php-ext-enable memcache \
;fi

###########################################################################
# mongodb:
###########################################################################

ARG INSTALL_MONGODB=false

RUN if [ ${INSTALL_MONGODB} = true ]; then \
    pecl install http://pecl.php.net/get/mongodb-1.8.0.tgz && \
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
# pcntl
###########################################################################

ARG INSTALL_PCNTL=false

RUN if [ ${INSTALL_PCNTL} = true ]; then \
    docker-php-ext-install pcntl \
;fi

###########################################################################
# pdo_mysql
###########################################################################

ARG INSTALL_PDO_MYSQL=false

RUN if [ ${INSTALL_PDO_MYSQL} = true ]; then \
    docker-php-ext-install pdo_mysql \
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
# sockets
###########################################################################

ARG INSTALL_SOCKETS=false

RUN if [ ${INSTALL_SOCKETS} = true ]; then \
    docker-php-ext-install sockets \
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