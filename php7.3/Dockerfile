FROM php:7.3-fpm-alpine

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
    && pecl install https://pecl.php.net/get/redis-5.0.2.tgz \
    && docker-php-ext-enable redis

# 安装 xdebug
RUN set -xe \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

# 安装 Mongo
RUN set -xe \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb

# 安装 Amqp
RUN set -xe \
    && apk --no-cache add rabbitmq-c-dev nghttp2-dev hiredis-dev \
    && pecl install http://pecl.php.net/get/amqp-1.9.4.tgz \
    && docker-php-ext-enable amqp

# 安装 Swoole
RUN set -xe \
    && apk --no-cache add linux-headers \
    && pecl install http://pecl.php.net/get/swoole-4.4.5.tgz \
    && docker-php-ext-enable swoole

RUN set -xe \
    && apk --no-cache add git

# 安装 xhprof
RUN set -xe \
	&& curl -L -o /tmp/xhprof.tar.gz "https://github.com/tideways/php-xhprof-extension/archive/v4.1.7.tar.gz" \
	&& mkdir -p xhprof \
	&& tar -C xhprof -zxvf /tmp/xhprof.tar.gz --strip 1 \
	&& ( \
		cd xhprof \
		&& phpize \
		&& ./configure \
		&& make \
		&& make install \
	) \
	&& rm -rf xhprof \
	&& rm /tmp/xhprof.tar.gz

# 安装 Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && unlink composer-setup.php \
    && mv composer.phar /usr/local/bin/composer \
    && /usr/local/bin/composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

# 清理
RUN rm -rf /usr/local/src/* \
    && rm -rf /tmp/pear/download/* \
    && rm -rf /tmp/pear/cache/*

EXPOSE 9000
EXPOSE 8888
EXPOSE 9501
EXPOSE 9502