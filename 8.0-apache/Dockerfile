ARG PHP_VERSION="8.0"
ARG COMPOSER_VERSION="2"

# ======================================================================================================================
#                                                   --- Composer ---
# ======================================================================================================================

FROM composer:${COMPOSER_VERSION} as composer

# ======================================================================================================================
#                                                   --- PHP ---
# ======================================================================================================================

FROM php:${PHP_VERSION}-apache

ENV TZ=Europe/Prague

RUN apt-get update \
    && apt-get install -y \
        wget \
        curl \
        apt-transport-https \
        ca-certificates \
        git \
        unzip \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime \
    && echo $TZ > /etc/timezone

# https://github.com/mlocati/docker-php-extension-installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions \
    && sync  \
    && install-php-extensions \
        intl \
        zip \
        pdo \
        pdo_mysql

# Clean
RUN apt-get clean -y  \
    && apt-get autoclean -y \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* /var/lib/log/* /tmp/* /var/tmp/*

# Composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Development config
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Custom config
COPY conf.d/custom.ini $PHP_INI_DIR/conf.d/999-custom.ini

# www
COPY ./www /var/www/html

WORKDIR /var/www/html