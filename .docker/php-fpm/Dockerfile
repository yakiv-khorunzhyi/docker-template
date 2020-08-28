FROM php:7.2.27-fpm-stretch

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Fix permissions
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

# Install selected extensions and other stuff
RUN apt-get update && apt-get -y --no-install-recommends install \
    apt-utils \
    libpq-dev \
    curl \
    wget \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    zlib1g-dev \
    libicu-dev \
    g++ \
    libmagickwand-dev \
    libxml2-dev \
    zip \
    unzip \
    && docker-php-ext-configure intl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install pdo_mysql mbstring mcrypt bcmath zip exif pcntl intl gd xml \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN pecl channel-update pecl.php.net \
    && pecl install xdebug imagick \
    && docker-php-ext-enable xdebug imagick

COPY --from=composer /usr/bin/composer /usr/bin/composer

# Copy xdebug configuration for remote debugging
COPY /.docker/php-fpm/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini