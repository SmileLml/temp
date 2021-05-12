#--------------------------------------------------------------------------
# Image Setup
#--------------------------------------------------------------------------

ARG PHP_VERSION

FROM phusion/baseimage:0.11

ARG PHP_VERSION

LABEL maintainer="liumingliang <liumingliang@qie.tv>"

RUN locale-gen en_US.UTF-8

###########################################################################
# Set Environment Variables
###########################################################################
ENV LANGUAGE=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV LC_CTYPE=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV TERM xterm
ENV PHP_VERSION=${PHP_VERSION}
ENV DEBIAN_FRONTEND noninteractive

USER root

###########################################################################
# Change Source
###########################################################################

ARG CHANGE_SOURCE=false
RUN if [ ${CHANGE_SOURCE} = true ]; then \
    sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
    sed -i 's/security-cdn.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list \
;fi

ARG CHANGE_SOURCE=false
ARG UBUNTU_SOURCE
COPY ./sources.sh /tmp/sources.sh

RUN if [ ${CHANGE_SOURCE} = true ]; then \
    chmod +x /tmp/sources.sh && \
    /bin/sh -c /tmp/sources.sh && \
    rm -rf /tmp/sources.sh \
;fi

###########################################################################
# Set Timezone
###########################################################################

ARG TIMEZONE=PRC
ENV TIMEZONE ${TIMEZONE}

RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

###########################################################################
# User Aliases
###########################################################################

COPY ./aliases.sh /root/aliases.sh

RUN sed -i 's/\r//' /root/aliases.sh && \
    echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source ~/aliases.sh" >> ~/.bashrc && \
	echo "" >> ~/.bashrc

###########################################################################
# Add PHP PPA
###########################################################################

RUN apt-get install -y software-properties-common && \
    add-apt-repository -y ppa:ondrej/php

###########################################################################
# Update Software
###########################################################################

RUN apt-get update -yqq && \
    apt-get upgrade -yqq

############################################################################
## PHP CLI
############################################################################

RUN echo 'DPkg::options { "--force-confdef"; };' >> /etc/apt/apt.conf
RUN apt-get install -y \
        --allow-downgrades \
        --allow-remove-essential \
        --allow-change-held-packages \
        php${PHP_VERSION}-cli \
        php${PHP_VERSION}-common

############################################################################
## Software Installation
############################################################################

RUN apt-get install -y \
        --allow-downgrades \
        --allow-remove-essential \
        --allow-change-held-packages \
        pkg-config \
        libcurl4-openssl-dev \
        libedit-dev \
        libssl-dev \
        libxml2-dev \
        libxslt-dev \
        libldap2-dev \
        libyaml-dev \
        libpng16-16 \
        libssl-dev \
        openssl \
        xz-utils \
        apt-utils \
        git \
        curl \
        vim \
        nano \
        libzip-dev \
        zip unzip \
        nasm

###########################################################################
# Oh My ZSH
###########################################################################

ARG SHELL_OH_MY_ZSH=false
RUN if [ ${SHELL_OH_MY_ZSH} = true ]; then \
    apt install -y zsh \
;fi

RUN if [ ${SHELL_OH_MY_ZSH} = true ]; then \
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh) --keep-zshrc" && \
    sed -i -r 's/^plugins=\(.*?\)$/plugins=(laravel6)/' /root/.zshrc && \
    echo '\n\
bindkey "^[OB" down-line-or-search\n\
bindkey "^[OC" forward-char\n\
bindkey "^[OD" backward-char\n\
bindkey "^[OF" end-of-line\n\
bindkey "^[OH" beginning-of-line\n\
bindkey "^[[1~" beginning-of-line\n\
bindkey "^[[3~" delete-char\n\
bindkey "^[[4~" end-of-line\n\
bindkey "^[[5~" up-line-or-history\n\
bindkey "^[[6~" down-line-or-history\n\
bindkey "^?" backward-delete-char\n' >> /root/.zshrc \
;fi

###########################################################################
# PHP EXTENSION BASE
###########################################################################

RUN apt-get install -y \
        --allow-downgrades \
        --allow-remove-essential \
        --allow-change-held-packages \
        php${PHP_VERSION}-apcu \
        php${PHP_VERSION}-curl \
        php${PHP_VERSION}-intl \
        php${PHP_VERSION}-json \
        php${PHP_VERSION}-xml \
        php${PHP_VERSION}-xsl \
        php${PHP_VERSION}-mbstring \
        php${PHP_VERSION}-mysql \
        php${PHP_VERSION}-pgsql \
        php${PHP_VERSION}-sqlite \
        php${PHP_VERSION}-sqlite3 \
        php${PHP_VERSION}-zip \
        php${PHP_VERSION}-bcmath \
        php${PHP_VERSION}-memcache \
        php${PHP_VERSION}-memcached \
        php${PHP_VERSION}-gd \
        php${PHP_VERSION}-dev \
        php${PHP_VERSION}-bz2 \
        php${PHP_VERSION}-gmp \
        php${PHP_VERSION}-imap \
        php${PHP_VERSION}-phpdbg \
        php${PHP_VERSION}-redis \
        php${PHP_VERSION}-yaml \
        php${PHP_VERSION}-mailparse \
        php${PHP_VERSION}-soap \
        php${PHP_VERSION}-uuid \
        php${PHP_VERSION}-wddx


###########################################################################
# SSH2 EXTENSION
###########################################################################

ARG INSTALL_SSH2=false

RUN if [ ${INSTALL_SSH2} = true ]; then \
  apt-get -y install libssh2-1-dev php${PHP_VERSION}-ssh2 \
;fi

###########################################################################
# LDAP EXTENSION
###########################################################################

ARG INSTALL_LDAP=false

RUN if [ ${INSTALL_LDAP} = true ]; then \
    apt-get install -y libldap2-dev && \
    apt-get install -y php${PHP_VERSION}-ldap \
;fi

###########################################################################
# SMB EXTENSION
###########################################################################

ARG INSTALL_SMB=false

RUN if [ ${INSTALL_SMB} = true ]; then \
    apt-get install -y smbclient php${PHP_VERSION}-smbclient coreutils \
;fi

###########################################################################
# xDebug EXTENSION
###########################################################################

USER root

ARG INSTALL_XDEBUG=false

RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    apt-get install -y php${PHP_VERSION}-xdebug && \
    sed -i 's/^;//g' /etc/php/${PHP_VERSION}/cli/conf.d/20-xdebug.ini \
;fi

COPY ./xdebug.ini /etc/php/${PHP_VERSION}/cli/conf.d/xdebug.ini

RUN sed -i "s/xdebug.remote_autostart=0/xdebug.remote_autostart=1/" /etc/php/${PHP_VERSION}/cli/conf.d/xdebug.ini && \
    sed -i "s/xdebug.remote_enable=0/xdebug.remote_enable=1/" /etc/php/${PHP_VERSION}/cli/conf.d/xdebug.ini && \
    sed -i "s/xdebug.cli_color=0/xdebug.cli_color=1/" /etc/php/${PHP_VERSION}/cli/conf.d/xdebug.ini

###########################################################################
# PECL UPDATE
###########################################################################

RUN pecl channel-update pecl.php.net

###########################################################################
# PCOV EXTENSION
###########################################################################

ARG INSTALL_PCOV=false

RUN if [ ${INSTALL_PCOV} = true ]; then \
    pecl install pcov && \
    echo "extension=pcov.so" >> /etc/php/${PHP_VERSION}/cli/php.ini && \
    echo "pcov.enabled" >> /etc/php/${PHP_VERSION}/cli/php.ini \
;fi

###########################################################################
# MongoDB EXTENSION
###########################################################################

ARG INSTALL_MONGODB=false

RUN if [ ${INSTALL_MONGODB} = true ]; then \
    pecl install mongodb && \
    echo "extension=mongodb.so" >> /etc/php/${PHP_VERSION}/mods-available/mongodb.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/mongodb.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-mongodb.ini \
;fi

###########################################################################
# AMQP EXTENSION
###########################################################################

ARG INSTALL_AMQP=false

RUN if [ ${INSTALL_AMQP} = true ]; then \
    apt-get install librabbitmq-dev -y && \
    pecl -q install amqp && \
    echo "extension=amqp.so" >> /etc/php/${PHP_VERSION}/mods-available/amqp.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/amqp.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-amqp.ini \
;fi

###########################################################################
# CASSANDRA EXTENSION
###########################################################################

ARG INSTALL_CASSANDRA=false

RUN if [ ${INSTALL_CASSANDRA} = true ]; then \
    apt-get install -y libgmp-dev re2c && \
    curl https://downloads.datastax.com/cpp-driver/ubuntu/18.04/dependencies/libuv/v1.28.0/libuv1-dev_1.28.0-1_amd64.deb -o libuv1-dev.deb && \
    curl https://downloads.datastax.com/cpp-driver/ubuntu/18.04/dependencies/libuv/v1.28.0/libuv1_1.28.0-1_amd64.deb -o libuv1.deb && \
    curl https://downloads.datastax.com/cpp-driver/ubuntu/18.04/cassandra/v2.12.0/cassandra-cpp-driver-dev_2.12.0-1_amd64.deb -o cassandra-cpp-driver-dev.deb && \
    curl https://downloads.datastax.com/cpp-driver/ubuntu/18.04/cassandra/v2.12.0/cassandra-cpp-driver_2.12.0-1_amd64.deb -o cassandra-cpp-driver.deb && \
    dpkg -i libuv1.deb && \
    dpkg -i libuv1-dev.deb && \
    dpkg -i cassandra-cpp-driver.deb && \
    dpkg -i cassandra-cpp-driver-dev.deb && \
    rm libuv1.deb libuv1-dev.deb cassandra-cpp-driver-dev.deb cassandra-cpp-driver.deb && \
    cd /usr/src && \
    git clone https://github.com/datastax/php-driver.git && \
    cd /usr/src/php-driver/ext && \
    phpize && \
    mkdir /usr/src/php-driver/build && \
    cd /usr/src/php-driver/build && \
    ../ext/configure > /dev/null && \
    make clean >/dev/null && \
    make >/dev/null 2>&1 && \
    make install && \
    echo "extension=cassandra.so" >> /etc/php/${PHP_VERSION}/mods-available/cassandra.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/cassandra.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-cassandra.ini \
;fi

###########################################################################
# Gearman EXTENSION
###########################################################################

ARG INSTALL_GEARMAN=false

RUN if [ ${INSTALL_GEARMAN} = true ]; then \
    add-apt-repository -y ppa:ondrej/pkg-gearman && \
    apt-get update && \
    apt-get install php${PHP_VERSION}-gearman -y  \
;fi

###########################################################################
# Swoole EXTENSION
###########################################################################

ARG INSTALL_SWOOLE=false

RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    pecl install swoole && \
    echo "extension=swoole.so" >> /etc/php/${PHP_VERSION}/mods-available/swoole.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/swoole.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-swoole.ini \
;fi

###########################################################################
# Taint EXTENSION
###########################################################################

ARG INSTALL_TAINT=false

RUN if [ "${INSTALL_TAINT}" = true ]; then \
    pecl install taint && \
    echo "extension=taint.so" >> /etc/php/${PHP_VERSION}/mods-available/taint.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/taint.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-taint.ini \
;fi

###########################################################################
# Inotify EXTENSION
###########################################################################

ARG INSTALL_INOTIFY=false

RUN if [ ${INSTALL_INOTIFY} = true ]; then \
    pecl -q install inotify && \
    echo "extension=inotify.so" >> /etc/php/${PHP_VERSION}/mods-available/inotify.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/inotify.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-inotify.ini \
;fi

###########################################################################
# PROTOBUF EXTENSION
###########################################################################

ARG INSTALL_PROTOBUF=false

RUN if [ ${INSTALL_PROTOBUF} = true ]; then \
    pecl -q install protobuf && \
    echo "extension=protobuf.so" >> /etc/php/${PHP_VERSION}/mods-available/protobuf.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/protobuf.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-protobuf.ini \
;fi

###########################################################################
# GRPC EXTENSION
###########################################################################

ARG INSTALL_GRPC=false

RUN if [ ${INSTALL_GRPC} = true ]; then \
    pecl -q install grpc && \
    echo "extension=grpc.so" >> /etc/php/${PHP_VERSION}/mods-available/grpc.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/grpc.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-grpc.ini \
;fi

###########################################################################
# AST EXTENSION
###########################################################################

ARG INSTALL_AST=false
ARG AST_VERSION=1.0.3
ENV AST_VERSION ${AST_VERSION}

RUN if [ ${INSTALL_AST} = true ]; then \
    printf "\n" | pecl -q install ast-${AST_VERSION} && \
    echo "extension=ast.so" >> /etc/php/${PHP_VERSION}/mods-available/ast.ini && \
    phpenmod -v ${PHP_VERSION} -s cli ast \
;fi

###########################################################################
# Phalcon EXTENSION
###########################################################################

ARG INSTALL_PHALCON=false
ARG PHALCON_VERSION
ENV PHALCON_VERSION ${PHALCON_VERSION}

RUN if [ $INSTALL_PHALCON = true ]; then \
    apt-get -y update && apt-get install -y unzip libpcre3-dev gcc make re2c && \
    curl -L -o /tmp/cphalcon.zip https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.zip && \
    unzip -d /tmp/ /tmp/cphalcon.zip && \
    cd /tmp/cphalcon-${PHALCON_VERSION}/build && \
    ./install && \
    echo "extension=phalcon.so" >> /etc/php/${PHP_VERSION}/mods-available/phalcon.ini && \
    ln -s /etc/php/${PHP_VERSION}/mods-available/phalcon.ini /etc/php/${PHP_VERSION}/cli/conf.d/30-phalcon.ini && \
    rm -rf /tmp/cphalcon* \
;fi

###########################################################################
# Aerospike EXTENSION
###########################################################################

ARG INSTALL_AEROSPIKE=false

RUN if [ ${INSTALL_AEROSPIKE} = true ]; then \
    apt-get -y install sudo wget && \
    curl -L -o /tmp/aerospike-client-php.tar.gz https://github.com/aerospike/aerospike-client-php/archive/master.tar.gz && \
    mkdir -p /tmp/aerospike-client-php && \
    tar -C /tmp/aerospike-client-php -zxvf /tmp/aerospike-client-php.tar.gz --strip 1 && \
    cd /tmp/aerospike-client-php/src && \
    phpize && \
    ./build.sh && \
    make install && \
    rm /tmp/aerospike-client-php.tar.gz && \
    echo 'extension=aerospike.so' >> /etc/php/${PHP_VERSION}/cli/conf.d/aerospike.ini && \
    echo 'aerospike.udf.lua_system_path=/usr/local/aerospike/lua' >> /etc/php/${PHP_VERSION}/cli/conf.d/aerospike.ini && \
    echo 'aerospike.udf.lua_user_path=/usr/local/aerospike/usr-lua' >> /etc/php/${PHP_VERSION}/cli/conf.d/aerospike.ini \
;fi

###########################################################################
# IonCube Loader EXTENSION
###########################################################################

ARG INSTALL_IONCUBE=false

RUN if [ ${INSTALL_IONCUBE} = true ]; then \
    curl -L -o /tmp/ioncube_loaders_lin_x86-64.tar.gz https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz && \
    tar zxpf /tmp/ioncube_loaders_lin_x86-64.tar.gz -C /tmp && \
    mv /tmp/ioncube/ioncube_loader_lin_${PHP_VERSION}.so $(php -r "echo ini_get('extension_dir');")/ioncube_loader.so && \
    echo "zend_extension=ioncube_loader.so" >> /etc/php/${PHP_VERSION}/mods-available/0ioncube.ini && \
    rm -rf /tmp/ioncube* \
;fi

###########################################################################
# Composer
###########################################################################

RUN curl -s http://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

COPY ./composer.json /root/.composer/composer.json

COPY ./auth.json /root/.composer/auth.json

ARG COMPOSER_GLOBAL_INSTALL=false
ENV COMPOSER_GLOBAL_INSTALL ${COMPOSER_GLOBAL_INSTALL}

RUN if [ ${COMPOSER_GLOBAL_INSTALL} = true ]; then \
    composer global install \
;fi

ARG COMPOSER_AUTH=false
ENV COMPOSER_AUTH ${COMPOSER_AUTH}

RUN if [ ${COMPOSER_AUTH} = false ]; then \
    rm /root/.composer/auth.json \
;fi

ARG COMPOSER_REPO_PACKAGIST
ENV COMPOSER_REPO_PACKAGIST ${COMPOSER_REPO_PACKAGIST}

RUN if [ ${COMPOSER_REPO_PACKAGIST} ]; then \
    composer config -g repo.packagist composer ${COMPOSER_REPO_PACKAGIST} \
;fi

RUN echo "" >> ~/.bashrc && \
    echo 'export PATH="~/.composer/vendor/bin:$PATH"' >> ~/.bashrc

RUN . ~/.bashrc

###########################################################################
# Laravel Envoy
###########################################################################

ARG INSTALL_LARAVEL_ENVOY=false

RUN if [ ${INSTALL_LARAVEL_ENVOY} = true ]; then \
    composer global require "laravel/envoy=~1.0" \
;fi

###########################################################################
# Laravel Installer
###########################################################################

ARG INSTALL_LARAVEL_INSTALLER=false

RUN if [ ${INSTALL_LARAVEL_INSTALLER} = true ]; then \
	composer global require "laravel/installer" \
;fi

###########################################################################
# Prestissimo
###########################################################################

ARG INSTALL_PRESTISSIMO=false

RUN if [ ${INSTALL_PRESTISSIMO} = true ]; then \
    composer global require "hirak/prestissimo" \
;fi

###########################################################################
# ping:
###########################################################################

ARG INSTALL_PING=false

RUN if [ ${INSTALL_PING} = true ]; then \
    apt-get update -yqq && \
    apt-get -y install inetutils-ping \
;fi

###########################################################################
# sshpass:
###########################################################################

ARG INSTALL_SSHPASS=false

RUN if [ ${INSTALL_SSHPASS} = true ]; then \
    apt-get update -yqq && \
    apt-get -y install sshpass \
;fi


###########################################################################
# GNU Parallel:
###########################################################################

ARG INSTALL_GNU_PARALLEL=false

RUN if [ ${INSTALL_GNU_PARALLEL} = true ]; then \
    apt-get -y install parallel \
;fi

###########################################################################
# Deployer:
###########################################################################

ARG INSTALL_DEPLOYER=false

RUN if [ ${INSTALL_DEPLOYER} = true ]; then \
    curl -LO https://deployer.org/deployer.phar && \
    mv deployer.phar /usr/local/bin/dep && \
    chmod +x /usr/local/bin/dep \
;fi

###########################################################################
# Minio:
###########################################################################

COPY mc/config.json /root/.mc/config.json

ARG INSTALL_MC=false

RUN if [ ${INSTALL_MC} = true ]; then\
    curl -fsSL -o /usr/local/bin/mc https://dl.minio.io/client/mc/release/linux-amd64/mc && \
    chmod +x /usr/local/bin/mc \
;fi

###########################################################################
# Symfony:
###########################################################################

ARG INSTALL_SYMFONY=false

RUN if [ ${INSTALL_SYMFONY} = true ]; then \
    mkdir -p /usr/local/bin && \
    curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony && \
    chmod a+x /usr/local/bin/symfony && \
    echo 'alias dev="php bin/console -e=dev"' >> ~/.bashrc && \
    echo 'alias prod="php bin/console -e=prod"' >> ~/.bashrc \
;fi

###########################################################################
# PYTHON:
###########################################################################

ARG INSTALL_PYTHON=false

RUN if [ ${INSTALL_PYTHON} = true ]; then \
    apt-get -y install python3 python3-pip && \
    python3 -m pip install --upgrade pip && \
    python3 -m pip install --upgrade virtualenv && \
    python3 -m pip install --upgrade virtualenv \
;fi

###########################################################################
# Drupal Console:
###########################################################################

ARG INSTALL_DRUPAL_CONSOLE=false

RUN if [ ${INSTALL_DRUPAL_CONSOLE} = true ]; then \
    apt-get -y install mysql-client && \
    curl https://drupalconsole.com/installer -L -o drupal.phar && \
    mv drupal.phar /usr/local/bin/drupal && \
    chmod +x /usr/local/bin/drupal \
;fi

###########################################################################
# SUPERVISOR:
###########################################################################

ARG INSTALL_SUPERVISOR=false

RUN if [ ${INSTALL_SUPERVISOR} = true ]; then \
    if [ ${INSTALL_PYTHON} = true ]; then \
        python3 -m pip install --upgrade supervisor && \
        echo_supervisord_conf > /etc/supervisord.conf && \
        sed -i 's/\;\[include\]/\[include\]/g' /etc/supervisord.conf && \
        sed -i 's/\;files\s.*/files = supervisord.d\/*.conf/g' /etc/supervisord.conf \
    ;fi \
;fi

###########################################################################
# Drush:
###########################################################################

ARG INSTALL_DRUSH=false
ARG DRUSH_VERSION
ENV DRUSH_VERSION ${DRUSH_VERSION}

RUN if [ ${INSTALL_DRUSH} = true ]; then \
    apt-get -y install mysql-client && \
    curl -fsSL -o /usr/local/bin/drush https://github.com/drush-ops/drush/releases/download/${DRUSH_VERSION}/drush.phar | bash && \
    chmod +x /usr/local/bin/drush && \
    drush core-status \
;fi

###########################################################################
# Linuxbrew:
###########################################################################

ARG INSTALL_LINUXBREW=false

RUN if [ ${INSTALL_LINUXBREW} = true ]; then \
    apt-get upgrade -y && \
    apt-get install -y build-essential make cmake scons curl git \
        ruby autoconf automake autoconf-archive \
        gettext libtool flex bison \
        libbz2-dev libcurl4-openssl-dev \
        libexpat-dev libncurses-dev && \
    git clone --depth=1 https://github.com/Homebrew/linuxbrew.git ~/.linuxbrew && \
    echo "" >> ~/.bashrc && \
    echo 'export PKG_CONFIG_PATH"=/usr/local/lib/pkgconfig:/usr/local/lib64/pkgconfig:/usr/lib64/pkgconfig:/usr/lib/pkgconfig:/usr/lib/x86_64-linux-gnu/pkgconfig:/usr/lib64/pkgconfig:/usr/share/pkgconfig:$PKG_CONFIG_PATH"' >> ~/.bashrc && \
    echo 'export LINUXBREWHOME="$HOME/.linuxbrew"' >> ~/.bashrc && \
    echo 'export PATH="$LINUXBREWHOME/bin:$PATH"' >> ~/.bashrc && \
    echo 'export MANPATH="$LINUXBREWHOME/man:$MANPATH"' >> ~/.bashrc && \
    echo 'export PKG_CONFIG_PATH="$LINUXBREWHOME/lib64/pkgconfig:$LINUXBREWHOME/lib/pkgconfig:$PKG_CONFIG_PATH"' >> ~/.bashrc && \
    echo 'export LD_LIBRARY_PATH="$LINUXBREWHOME/lib64:$LINUXBREWHOME/lib:$LD_LIBRARY_PATH"' >> ~/.bashrc \
;fi

###########################################################################
# Dusk Dependencies:
###########################################################################

ARG CHROME_DRIVER_VERSION=stable
ENV CHROME_DRIVER_VERSION ${CHROME_DRIVER_VERSION}
ARG INSTALL_DUSK_DEPS=false

RUN if [ ${INSTALL_DUSK_DEPS} = true ]; then \
    apt-get -y install zip wget unzip xdg-utils \
        libxpm4 libxrender1 libgtk2.0-0 libnss3 libgconf-2-4 xvfb \
        gtk2-engines-pixbuf xfonts-cyrillic xfonts-100dpi xfonts-75dpi \
        xfonts-base xfonts-scalable x11-apps \
    && wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb \
    && dpkg -i --force-depends google-chrome-stable_current_amd64.deb \
    && apt-get -y -f install \
    && dpkg -i --force-depends google-chrome-stable_current_amd64.deb \
    && rm google-chrome-stable_current_amd64.deb \
    && wget https://chromedriver.storage.googleapis.com/${CHROME_DRIVER_VERSION}/chromedriver_linux64.zip \
    && unzip chromedriver_linux64.zip \
    && mv chromedriver /usr/local/bin/ \
    && rm chromedriver_linux64.zip \
;fi

###########################################################################
# Subversion:
###########################################################################

ARG INSTALL_SUBVERSION=false

RUN if [ ${INSTALL_SUBVERSION} = true ]; then \
    apt-get install -y subversion \
;fi

###########################################################################
# Blackfire:
###########################################################################

ARG INSTALL_BLACKFIRE=false
ARG BLACKFIRE_CLIENT_ID
ENV BLACKFIRE_CLIENT_ID ${BLACKFIRE_CLIENT_ID}
ARG BLACKFIRE_CLIENT_TOKEN
ENV BLACKFIRE_CLIENT_TOKEN ${BLACKFIRE_CLIENT_TOKEN}

RUN if [ ${INSTALL_XDEBUG} = false -a ${INSTALL_BLACKFIRE} = true ]; then \
    curl -L https://packages.blackfire.io/gpg.key | apt-key add - && \
    echo "deb http://packages.blackfire.io/debian any main" | tee /etc/apt/sources.list.d/blackfire.list && \
    apt-get update -yqq && \
    apt-get install blackfire-agent \
;fi

###########################################################################
# fswatch
###########################################################################

ARG INSTALL_FSWATCH=false

RUN if [ ${INSTALL_FSWATCH} = true ]; then \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 47FE03C1 \
    && add-apt-repository -y ppa:hadret/fswatch \
    || apt-get update -yqq \
    && apt-get -y install fswatch \
;fi

###########################################################################
# Bash Git Prompt
###########################################################################

ARG INSTALL_GIT_PROMPT=false

COPY git-prompt.sh /tmp/git-prompt

RUN if [ ${INSTALL_GIT_PROMPT} = true ]; then \
    git clone https://github.com/magicmonty/bash-git-prompt.git /root/.bash-git-prompt --depth=1 && \
    cat /tmp/git-prompt >> /root/.bashrc && \
    rm /tmp/git-prompt \
;fi

###########################################################################
## Final Touch
###########################################################################

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

WORKDIR /var/www