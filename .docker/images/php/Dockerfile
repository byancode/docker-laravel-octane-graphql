
FROM fedora:35 as builder

WORKDIR /var/www

ARG TIMEZONE UTC
RUN ln -snf /usr/share/zoneinfo/$TIMEZONE /etc/localtime && echo $TIMEZONE > /etc/timezone

RUN dnf -y update
RUN dnf -y install supervisor cronie
RUN dnf -y install http://rpms.remirepo.net/fedora/remi-release-35.rpm
RUN dnf -y install dnf-plugins-core
RUN dnf config-manager --set-enabled remi
RUN dnf module reset php
ARG PHP_VERSION 8.0
RUN dnf -y module install php:remi-${PHP_VERSION}
RUN dnf -y install \
    php \
    php-cli \
    php-fpm \
    php-pgsql \
    php-mysqlnd \
    php-gd \
    php-zip \
    php-devel \
    php-mcrypt \
    php-mbstring \
    php-memcache \
    php-imagick \
    php-opcache \
    php-mongodb \
    php-swoole \
    php-bcmath \
    php-xdebug \
    php-redis \
    php-curl \
    php-pear \
    php-json
RUN dnf -y install \
    php-xmlrpc
RUN dnf -y install unzip
RUN dnf clean all

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
ARG ROADRUNNER_VERSION latest
COPY --from=spiralscout/roadrunner:${ROADRUNNER_VERSION} /usr/bin/rr /usr/bin/rr

# PHP Basic Config

ARG PHP_MAX_EXECUTION_TIME      600
RUN sed -e 's/max_execution_time = 30/max_execution_time = '$PHP_MAX_EXECUTION_TIME'/' -i /etc/php.ini
ARG PHP_UPLOAD_MAX_FILESIZE     1G
RUN sed -e 's/upload_max_filesize = 2M/upload_max_filesize = '$PHP_UPLOAD_MAX_FILESIZE'/' -i /etc/php.ini
ARG PHP_MEMORY_LIMIT            512M
RUN sed -e 's/memory_limit = 128M/memory_limit = '$PHP_MEMORY_LIMIT'/' -i /etc/php.ini
ARG PHP_POST_MAX_SIZE           8M
RUN sed -e 's/post_max_size = 8M/post_max_size = '$PHP_POST_MAX_SIZE'/' -i /etc/php.ini

COPY ./schedule /etc/cron.d/schedule
COPY ./opcache.ini /etc/php.d/*-opcache.ini
COPY ./opcache.blacklist /etc/php.d/opcache-default.blacklist
COPY ./supervisord.conf /etc/supervisord.conf
COPY ./entrypoint /usr/local/bin/entrypoint

ARG ENABLE_OPCACHE 1
RUN sed -e 's/opcache.enable=0/opcache.enable='$ENABLE_OPCACHE'/' -i /etc/php.d/*-opcache.ini
RUN sed -e 's/opcache.enable_cli=0/opcache.enable_cli='$ENABLE_OPCACHE'/' -i /etc/php.d/*-opcache.ini

RUN chmod +x /usr/local/bin/entrypoint
RUN chmod +x /etc/cron.d/schedule
RUN chmod +x /usr/bin/rr

ARG OCTANE_SERVER    swoole
RUN sed -e 's/server=swoole/server='$OCTANE_SERVER'/' -i /etc/supervisord.conf

CMD ["php-fpm"]

EXPOSE 9000


FROM builder AS build1

WORKDIR /var/www

ENTRYPOINT [ "/usr/local/bin/entrypoint" ]

EXPOSE 8000
