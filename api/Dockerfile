FROM composer AS build
WORKDIR /build
COPY . .
RUN composer install --optimize-autoloader

FROM node:stretch AS node
WORKDIR /node
COPY --from=build /build .
RUN npm install && npm run build

FROM php:cli AS optimize
WORKDIR /build
COPY --from=node /build .
RUN php artisan config:cache \
    && php artisan route:cache

FROM php:fpm
WORKDIR /api
COPY php.ini /usr/local/etc/php/
COPY --from=optimize /build .
RUN export CFLAGS="$PHP_CFLAGS" CPPFLAGS="$PHP_CPPFLAGS" LDFLAGS="$PHP_LDFLAGS" \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
    libmagickwand-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt mbstring pdo_mysql \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) opcache \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && rm -rf /var/lib/apt/lists/*
