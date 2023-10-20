FROM php:7.3-fpm-alpine

#本機即時檔案變動需求，線上不用
# RUN apt-get install -y npm
# RUN npm install --save-dev chokidar
#

# Configure non-root user.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN apk --no-cache add shadow && \
    groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data -s /bin/bash

RUN apk update && apk add  \
      freetype-dev  \
      libjpeg-turbo-dev  \
      wget  \
      git  \
      bash  \
      libressl-dev \
      libzip-dev \
      zip \
      libpng \
      libpng-dev \
      autoconf \
      build-base \
    && apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
    && pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis \
    && pecl install zip \
    && docker-php-ext-enable zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr --with-jpeg-dir=/usr --with-png-dir=/usr \
    && docker-php-ext-install bcmath gd ctype mysqli pdo_mysql pcntl gd \
    && rm -f swoole.tar.gz $HOME/.composer/*-old.phar \
    && docker-php-source delete \
    && apk del .phpize-deps libpng-dev

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#DEBUG工具
RUN apk add iputils npm
#net-tools

COPY *.sh /etc/profile.d/

USER root

RUN echo '\
        . /etc/profile ; \
    ' >> /root/.bashrc

USER www-data

RUN echo '\
        . /etc/profile ; \
    ' >> ~/.bashrc

WORKDIR /var/www

USER root

CMD ["php-fpm"]

EXPOSE 9000