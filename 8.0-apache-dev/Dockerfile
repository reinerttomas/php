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

ENV TIMEZONE=Europe/Prague

RUN apt-get update \
    && apt-get -y --no-install-recommends install wget \
    curl \
    apt-transport-https \
    ca-certificates \
    git \
    unzip \
    && ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime \
    && echo $TIMEZONE > /etc/timezone

# https://github.com/mlocati/docker-php-extension-installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && sync

RUN install-php-extensions intl \
    zip \
    pdo \
    pdo_mysql \
    mysqli

# Clean
RUN rm -rf /var/lib/apt/lists/* /var/lib/log/* /tmp/* /var/tmp/*

# Development config
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Custom config
COPY conf.d/custom.ini $PHP_INI_DIR/conf.d/999-custom.ini

# Composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html