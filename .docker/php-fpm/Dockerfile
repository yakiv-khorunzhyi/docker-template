FROM php:7.2.27-fpm-stretch

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Fix permissions
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

# Install selected extensions and other stuff
RUN apt-get update && apt-get -y --no-install-recommends install \
    libpq-dev \
    && docker-php-ext-install cli bcmath posix pcntl hash iconv curl pdo_mysql json mbstring mcrypt bcmath bz2 zip exif pcntl xml xmlreader xmlwriter zip \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN pecl channel-update pecl.php.net \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

COPY --from=composer /usr/bin/composer /usr/bin/composer