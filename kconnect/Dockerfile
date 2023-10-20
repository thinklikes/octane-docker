FROM phpswoole/swoole:4.8-php8.1-alpine

RUN apk update && apk add wget git bash libressl-dev libzip-dev zip \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    && rm -rf /tmp/pear \
    && pecl install zip \
    && docker-php-ext-install bcmath ctype pdo_mysql pcntl \
    && rm -f swoole.tar.gz $HOME/.composer/*-old.phar \
    && docker-php-source delete \
    && apk del .phpize-deps

RUN docker-php-ext-install mysqli

#DEBUG工具
RUN apk add iputils npm
#net-tools

#COPY aliases.sh /etc/profile.d/

RUN echo '\
        . /etc/profile ; \
    ' >> /root/.bashrc

RUN mkdir /app
WORKDIR /app

CMD ["php", "-S", "0.0.0.0:8080"]