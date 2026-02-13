FROM php:8.2-cli

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ENV PATH="$PATH:/usr/local/bin"

RUN docker-php-ext-install pdo pdo_mysql

RUN apt-get update && apt-get install -y git zip && rm -rf /var/lib/apt/lists/*

RUN pecl install xdebug && docker-php-ext-enable xdebug
