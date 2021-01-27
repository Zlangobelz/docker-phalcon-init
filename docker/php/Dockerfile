FROM php:7.2-fpm

ARG USER_ID=1000
ARG GROUP_ID=1000

RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    libpng-dev \
    libicu-dev \
    libpcre3-dev \
    libxml2-dev

RUN apt update && docker-php-ext-install \
    mbstring \
    gettext \
    gd \
    fileinfo \
    json \
    intl \
    xml \
    zip

RUN wget https://getcomposer.org/installer -O - -q \
    | php -- --install-dir=/bin --filename=composer --quiet

ARG PSR_VERSION=0.7.0
ARG PHALCON_VERSION=4.1.0
ARG PHALCON_EXT_PATH=php7/64bits

RUN set -xe && \
    # Download PSR, see https://github.com/jbboehr/php-psr
    curl -LO https://github.com/jbboehr/php-psr/archive/v${PSR_VERSION}.tar.gz && \
    tar xzf ${PWD}/v${PSR_VERSION}.tar.gz && \
    # Download Phalcon
    curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz && \
    tar xzf ${PWD}/v${PHALCON_VERSION}.tar.gz && \
    docker-php-ext-install -j $(getconf _NPROCESSORS_ONLN) \
        ${PWD}/php-psr-${PSR_VERSION} \
        ${PWD}/cphalcon-${PHALCON_VERSION}/build/${PHALCON_EXT_PATH} \
    && \
    # Remove all temp files
    rm -r \
        ${PWD}/v${PSR_VERSION}.tar.gz \
        ${PWD}/php-psr-${PSR_VERSION} \
        ${PWD}/v${PHALCON_VERSION}.tar.gz \
        ${PWD}/cphalcon-${PHALCON_VERSION}


USER "${USER_ID}:${GROUP_ID}"

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000