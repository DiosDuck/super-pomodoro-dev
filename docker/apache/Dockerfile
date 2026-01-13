FROM php:8.2-apache

RUN a2enmod rewrite

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

COPY .htaccess /var/www/html/.htaccess

RUN apt-get update && apt-get install -y --no-install-recommends \
    libicu-dev \
    libzip-dev \
  && docker-php-ext-install -j"$(nproc)" \
    pdo_mysql \
    intl \
    zip \
    opcache \
  && rm -rf /var/lib/apt/lists/*
