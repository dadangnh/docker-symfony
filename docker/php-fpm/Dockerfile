FROM alpine:3.8

LABEL maintainer="Vincent Composieux <vincent.composieux@gmail.com>"

RUN rm -rf /var/cache/apk/* && \
    rm -rf /tmp/*

RUN apk update

RUN apk add --update \
    php7-fpm \
    php7-apcu \
    php7-ctype \
    php7-curl \
    php7-dom \
    php7-gd \
    php7-iconv \
    php7-imagick \
    php7-json \
    php7-intl \
    php7-mcrypt \
    php7-fileinfo\
    php7-mbstring \
    php7-opcache \
    php7-openssl \
    php7-pdo \
    php7-pdo_mysql \
    php7-pdo_sqlite \
    php7-pdo_pgsql \
    php7-pgsql \
    php7-mysqli \
    php7-xml \
    php7-zlib \
    php7-phar \
    php7-tokenizer \
    php7-session \
    php7-simplexml \
    php7-xdebug \
    php7-zip \
    php7-memcached \
    php7-sqlite3 \
    php7-sodium \
    php7-redis \
    php7-mongodb \
    make \
    curl

RUN rm -rf /var/cache/apk/* && rm -rf /tmp/* && \
    curl --insecure https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer

ADD symfony.ini /etc/php7/conf.d/
ADD symfony.ini /etc/php7/cli/conf.d/
ADD xdebug.ini  /etc/php7/conf.d/
ADD php.ini     /etc/php7/php.ini
RUN echo "include=/etc/php7/symfony.pool.conf" >> /etc/php7/php-fpm.conf
ADD symfony.pool.conf /etc/php7/

CMD ["php-fpm7", "-F"]

WORKDIR /var/www/symfony
EXPOSE 9001
