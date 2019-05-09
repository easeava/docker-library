FROM php:5.4.40-fpm

MAINTAINER tthd@163.com

RUN set +e; apt-get update || true && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev
RUN set -ex; docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd pdo_mysql

STOPSIGNAL SIGQUIT

EXPOSE 9000
CMD ["php-fpm"]