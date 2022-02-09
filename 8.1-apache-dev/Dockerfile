ARG PHP_VERSION="8.1"

FROM reinerttomas/php:${PHP_VERSION}-apache

LABEL maintainer="reinerttomas@gmail.com"

# PHP EXTENSIONS
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# INSTALLATION
RUN apt update && apt dist-upgrade -y && \
    # PHP EXTENSIONS
    chmod +x /usr/local/bin/install-php-extensions && \
        install-php-extensions @composer && \
    # CLEAN UP
    apt-get clean -y && \
    apt-get autoclean -y && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* /var/lib/log/* /tmp/* /var/tmp/*

# Production config
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Custom config
COPY conf.d/custom.ini $PHP_INI_DIR/conf.d/999-custom.ini