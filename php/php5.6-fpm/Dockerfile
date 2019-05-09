FROM php:5.6.11-fpm

MAINTAINER tthd@163.com

# RUN set -ex; rm -rf /etc/apt/sources.list && touch /etc/apt/sources.list

# RUN echo $'deb http://mirrors.163.com/debian/ jessie main non-free contrib\n\
# deb http://mirrors.163.com/debian/ jessie-updates main non-free contrib\n\
# deb http://mirrors.163.com/debian/ jessie-backports main non-free contrib\n\
# deb-src http://mirrors.163.com/debian/ jessie main non-free contrib\n\
# deb-src http://mirrors.163.com/debian/ jessie-updates main non-free contrib\n\
# deb-src http://mirrors.163.com/debian/ jessie-backports main non-free contrib\n\
# deb http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib\n\
# deb-src http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib\n'\
# >> /etc/example.conf

RUN set +e; apt-get update || true && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev
RUN set -ex; docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd pdo_mysql

RUN pecl install redis  \
    && pecl install mongodb \
    && docker-php-ext-enable redis mongodb

STOPSIGNAL SIGQUIT

EXPOSE 9000
CMD ["php-fpm"]