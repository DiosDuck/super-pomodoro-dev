FROM php:8.4-fpm-alpine

RUN apk add --no-cache bash curl ca-certificates

COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer

RUN apk add --no-cache icu-dev libzip-dev rabbitmq-c-dev openssl $PHPIZE_DEPS; \
    docker-php-ext-install -j"$(nproc)" pdo pdo_mysql intl zip opcache openssl; \
    pecl install redis; docker-php-ext-enable redis; \
    pecl install amqp; docker-php-ext-enable amqp; \
    pecl clear-cache; \
    apk del $PHPIZE_DEPS

WORKDIR /var/www/backend

CMD ["bash", "-c", "composer install && php-fpm -F"]
