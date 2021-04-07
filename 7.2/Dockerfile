# Based on the work of:
# Mr. Krzysztof Kawalec <kf.kawalec@gmail.com> (https://github.com/kfkawalec/gitlab-ci-laravel-php)
FROM php:7.3
MAINTAINER Pedro Hidalgo <xphidalgo@gmail.com>

RUN rm /etc/apt/preferences.d/no-debian-php && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
        openssh-client \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libcurl4-openssl-dev \
        libldap2-dev \
        libicu-dev \
        libc-client-dev \
        libkrb5-dev \
        libmagickwand-dev --no-install-recommends \
        curl \
        libtidy* \
        gnupg \
        git \
        libxml2-dev \
        php-soap \
        libpq-dev \
        python3 \
        python3-pip \
        gettext-base \
        libzip-dev \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*

RUN python3 -m pip install -U pip

# PIP packages
RUN pip3 install setuptools && pip3 install wheel && pip3 install awsebcli && pip3 install awscli

# PHP Extensions
RUN docker-php-ext-install \
        mbstring \
        curl \
        json \
        exif \
        tidy \
        zip \
        opcache \
        soap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu \
    && docker-php-ext-install ldap \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap \
    && pecl install imagick \
    && docker-php-ext-enable imagick

# Xdebug
RUN pecl install -o -f xdebug \ 
    && rm -rf /tmp/* \ 
    && docker-php-ext-enable xdebug 

# NodeJS
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean
  
# Memory Limit
RUN echo "memory_limit=-1" > $PHP_INI_DIR/conf.d/memory-limit.ini

# Time Zone
RUN echo "date.timezone=Europe/Warsaw" > $PHP_INI_DIR/conf.d/date_timezone.ini

VOLUME /root/composer

# Environmental Variables
ENV COMPOSER_HOME /root/composer

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
	composer selfupdate --1
    
# Goto temporary directory.
#WORKDIR /tmp

# Run composer and phpunit installation.
#RUN composer require "phpunit/phpunit=5.*" --prefer-source --no-interaction && \
#    ln -s /tmp/vendor/bin/phpunit /usr/local/bin/phpunit

# Run composer and codesniffer installation.
#RUN composer require "squizlabs/php_codesniffer=*" --prefer-source --no-interaction && \
#    ln -s /tmp/vendor/bin/phpcs /usr/local/bin/phpcs

RUN php --version
RUN composer --version
#RUN phpunit --version
#RUN phpcs --version
