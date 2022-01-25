FROM php:fpm-alpine3.15
LABEL maintainer="Emesh <easeava@gmail.com>" version="1.0" license="MIT"

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

##
# ---------- env settings ----------
##
ENV SW_VERSION=${SW_VERSION:-"v4.8.6"} \
    COMPOSER_VERSION=${COMPOSER_VERSION:-"2.2.5"} \
    PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc libc-dev make php8-dev php8-pear pkgconf re2c pcre-dev pcre2-dev zlib-dev libtool automake"

# update
RUN set -ex \
    && apk update \
    # extension libaio linux-headers
    && apk add --no-cache libstdc++ openssl git bash \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS libaio-dev openssl-dev curl-dev libjpeg-turbo-dev libpng-dev libjpeg freetype-dev libzip-dev libmemcached-dev \
    # extenstions
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pcntl pdo_mysql mysqli bcmath opcache \
    && pecl install redis  \
    && pecl install mongodb \
    && pecl install igbinary \
    && pecl install msgpack \
    && pecl install memcached \
    && pecl install zip \
    && docker-php-ext-enable redis mongodb igbinary msgpack memcached zip \
    # download
    && cd /tmp \
    && curl -SL "https://github.com/swoole/swoole-src/archive/${SW_VERSION}.tar.gz" -o swoole.tar.gz \
    && ls -alh \
    # php extension:swoole
    && cd /tmp \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-openssl --enable-http2 --enable-swoole-curl --enable-swoole-json \
        && make -s -j$(nproc) && make install \
    ) \
    && echo "memory_limit=1G" > /usr/local/etc/php/conf.d/00_default.ini \
    && echo "opcache.enable_cli = 'On'" >> /usr/local/etc/php/conf.d/00_opcache.ini \
    && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/50_swoole.ini \
    && echo "swoole.use_shortname = 'Off'" >> /usr/local/etc/php/conf.d/50_swoole.ini \
    # install composer
    && wget -nv -O /usr/local/bin/composer https://github.com/composer/composer/releases/download/${COMPOSER_VERSION}/composer.phar \
    && chmod u+x /usr/local/bin/composer \
    # php info
    && php -v \
    && php -m \
    && php --ri swoole \
    && php --ri Zend\ OPcache \
    && composer \
    # ---------- clear works ----------
    && apk del .build-deps \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"
