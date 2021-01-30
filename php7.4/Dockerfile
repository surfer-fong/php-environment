FROM php:7.4-fpm-alpine

LABEL maintainer="<13416335317@163.com>"

# 安装基本扩展
RUN set -xe \
	&& apk update \
	&& apk upgrade \
	&& apk add --no-cache  --update --virtual .phpize-deps $PHPIZE_DEPS \
	&& docker-php-ext-install bcmath pdo_mysql mysqli pcntl sockets

# 安装 gd
RUN set -xe \
	&& apk add --no-cache freetype libpng libjpeg-turbo freetype-dev libpng-dev libjpeg-turbo-dev \
	&& docker-php-ext-install gd

# 安装 zip
RUN set -xe \
	&& apk add --no-cache zip libzip-dev \
	&& docker-php-ext-configure zip \
	&& docker-php-ext-install zip

# 安装 redis
RUN set -xe \
	&& pecl install https://pecl.php.net/get/redis-5.3.2.tgz \
	&& docker-php-ext-enable redis

# 安装 xdebug
RUN set -xe \
	&& pecl install xdebug \
	&& docker-php-ext-enable xdebug

# 安装 mongo
RUN set -xe \
	&& pecl install mongodb \
	&& docker-php-ext-enable mongodb

# 安装 amqp
RUN set -xe \
	&& apk add --no-cache rabbitmq-c-dev nghttp2-dev hiredis-dev \
	&& pecl install http://pecl.php.net/get/amqp-1.10.2.tgz \
	&& docker-php-ext-enable amqp

# 安装 xhprof
RUN set -xe \
	&& apk add git \
	&& curl -fsSL -o /tmp/xhprof.tar.gz "https://github.com/tideways/php-xhprof-extension/archive/v5.0.2.tar.gz" \
	&& mkdir -p xhprof \
	&& tar -C xhprof -zxvf /tmp/xhprof.tar.gz --strip 1 \
	&& (cd xhprof && phpize && ./configure && make && make install) \
	&& rm -rf xhprof \
	&& rm /tmp/xhprof.tar.gz

# 安装 composer
RUN set -xe \
	&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
	&& php composer-setup.php \
	&& unlink composer-setup.php \
	&& mv composer.phar /usr/local/bin/composer \
	&& composer self-update --2 \
	&& /usr/local/bin/composer config -g repo.packagist composer https://mirror.aliyun.com/composer/

# 清理
RUN rm -rf /usr/local/src/* \
	&& rm -rf /tmp/pear/download/* \
	&& rm -rf /tmp/pear/cache/*

EXPOSE 9000