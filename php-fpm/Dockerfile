FROM php:7.4-fpm-alpine

#本機即時檔案變動需求，線上不用
# RUN apt-get install -y npm
# RUN npm install --save-dev chokidar
#

RUN apk update && apk add wget git bash libressl-dev libzip-dev zip libpng libpng-dev autoconf build-base \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    && pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && pecl install zip \
    && docker-php-ext-enable zip \
    && docker-php-ext-install bcmath gd ctype pdo_mysql pcntl \
    && rm -f swoole.tar.gz $HOME/.composer/*-old.phar \
    && docker-php-source delete \
    && apk del .phpize-deps libpng-dev

# RUN pecl install mongodb && docker-php-ext-enable mongodb

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#DEBUG工具
RUN apk add iputils npm
#net-tools

COPY aliases.sh /etc/profile.d/

RUN echo '\
        . /etc/profile ; \
    ' >> /root/.bashrc

RUN mkdir /app
WORKDIR /app

#CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]