FROM php:5.6-fpm-alpine

LABEL maintainer="吴泽丰<13416335317@163.com>"

# 安装基本扩展
RUN set -xe \
    && apk add --no-cache --update --virtual .phpize-deps $PHPIZE_DEPS \
    && docker-php-ext-install bcmath pdo_mysql mysqli pcntl sockets

# 安装 Gd
RUN set -xe \
    && apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
    && docker-php-ext-install gd

# 安装 Zip
RUN set -xe \
    && apk add --no-cache zip libzip-dev \
    && docker-php-ext-configure zip --with-libzip=/usr/include \
    && docker-php-ext-install zip


# 安装 Redis
RUN set -xe \
    && pecl install https://pecl.php.net/get/redis-2.2.8.tgz \
    && docker-php-ext-enable redis

# 安装 Xdebug
RUN set -xe \
    && pecl install xdebug-2.5.5 \
    && docker-php-ext-enable xdebug

# 安装 Mongo
RUN set -xe \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb

# 安装 Mongo
RUN set -xe \
    && pecl install https://pecl.php.net/get/mongo-1.6.0.tgz \
    && docker-php-ext-enable mongo

# 安装 Amqp
RUN set -xe \
    && apk --no-cache add rabbitmq-c-dev nghttp2-dev hiredis-dev \
    && pecl install http://pecl.php.net/get/amqp-1.6.0.tgz \
    && docker-php-ext-enable amqp

# 安装 Swoole
RUN set -xe \
    && apk --no-cache add linux-headers \
    && pecl install http://pecl.php.net/get/swoole-2.0.11.tgz \
    && docker-php-ext-enable swoole

# 安装 xhprof
RUN set -xe \
    && pecl install xhprof-0.9.4 \
    && docker-php-ext-enable xhprof

# 安装 Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && unlink composer-setup.php

# 安装 Python 和 graphviz
RUN set -xe \
    && apk --no-cache add python3 graphviz

# 清理
RUN rm -rf /usr/local/src/* \
    && rm -rf /tmp/pear/download/* \
    && rm -rf /tmp/pear/cache/*

EXPOSE 9000