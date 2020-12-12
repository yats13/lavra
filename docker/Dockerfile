FROM php:7.4-alpine

RUN apk --update add --no-cache --virtual .build-deps $PHPIZE_DEPS \
 && apk --update add --no-cache git bash curl
RUN pecl install xdebug apcu \
 && docker-php-ext-install opcache \
 && docker-php-ext-enable apcu xdebug \
 && apk del -f .build-deps

WORKDIR /var/www/html

ADD docker/opcache.ini /usr/local/etc/php/conf.d/opcache.ini
ADD docker/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony
RUN symfony server:ca:install

RUN git config --global user.email "yatskovsky1985@gmail.com"
RUN git config --global user.name "Andrey Yatskovsky"

CMD ["symfony", "serve"] 