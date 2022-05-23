FROM php:8.1-cli-alpine3.15

#DEBUG
# RUN apt-get install -y iputils-ping net-tools

#本機即時檔案變動需求，線上不用
# RUN apt-get install -y npm
# RUN npm install --save-dev chokidar
#

RUN apk update && apk add wget git libressl-dev libzip-dev zip \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    && pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && docker-php-ext-install zip \
    && docker-php-ext-install bcmath ctype pdo_mysql pcntl \
    && rm -f swoole.tar.gz $HOME/.composer/*-old.phar \
    && docker-php-source delete \
    && apk del .phpize-deps

RUN mkdir /octane
WORKDIR /octane

#RUN echo "alias art='php artisan'" >> ~/.bashrc
#RUN echo "alias ostart='php artisan octane:start --host=0.0.0.0'" >> ~/.bashrc

CMD ["php", "artisan", "serve", "--host=0.0.0.0"]