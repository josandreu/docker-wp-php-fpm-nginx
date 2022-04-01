# https://hub.docker.com/_/wordpress/
FROM wordpress:5.9.2-php7.4-fpm-alpine

# Since this Dockerfile extends the official Docker image `wordpress`,
# and since `wordpress`, in turn, extends the official Docker image `php`,
# the helper script docker-php-ext-enable (defined for image `php`)
# works here, and we can use it to enable xdebug:

RUN apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apk del -f .build-deps

# Copy xdebug.ini to /usr/local/etc/php/conf.d/
COPY ./config/xdebug.ini /usr/local/etc/php/conf.d/

EXPOSE 80
EXPOSE 443
