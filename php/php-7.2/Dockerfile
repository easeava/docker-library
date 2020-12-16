FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd pdo_mysql mysqli bcmath exif

RUN pecl install redis  \
    && pecl install mongodb \
    && docker-php-ext-enable redis mongodb

# COPY swoole_loader72.so /usr/local/lib/php/extensions/no-debug-non-zts-20170718
# COPY docker-php-ext-swoole_loader.ini /usr/local/etc/php/conf.d

# RUN usermod -u 1000 www-data && groupmod -g 1000 www-data
RUN mkdir -p /usr/share/nginx/html
WORKDIR /usr/share/nginx/html