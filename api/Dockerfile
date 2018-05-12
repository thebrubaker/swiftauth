FROM composer AS build
WORKDIR /build
COPY composer.json /build
COPY composer.lock /build
RUN composer install --no-scripts --no-autoloader

FROM composer AS bootstrap
WORKDIR /build
COPY . /build
COPY --from=build /build/composer.json /build/composer.json
COPY --from=build /build/composer.lock /build/composer.lock
COPY --from=build /build/vendor /build/vendor
RUN composer dump-autoload --optimize

FROM thebrubaker/laravel-php:latest
WORKDIR /www
COPY --from=bootstrap /build /www