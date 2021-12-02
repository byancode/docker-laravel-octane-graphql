ARG PHP_VERSION
FROM fedora:35 as builder

WORKDIR /var/www

ENV TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN dnf -y update
RUN dnf -y install supervisor cronie
RUN dnf -y install http://rpms.remirepo.net/fedora/remi-release-35.rpm
RUN dnf -y install dnf-plugins-core
RUN dnf config-manager --set-enabled remi
RUN dnf module reset php
ARG PHP_VERSION
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
RUN dnf -y install nodejs unzip
RUN dnf clean all

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
COPY --from=spiralscout/roadrunner:2.4.2 /usr/bin/rr /usr/bin/rr

RUN sed -e 's/max_execution_time = 30/max_execution_time = 600/' -i /etc/php.ini
RUN sed -e 's/memory_limit = 128M/memory_limit = 512M/' -i /etc/php.ini

COPY ./schedule /etc/cron.d/schedule
COPY ./opcache.ini /etc/php.d/*-opcache.ini
COPY ./opcache.blacklist /etc/php.d/opcache-default.blacklist
COPY ./supervisord.conf /etc/supervisord.conf
COPY ./entrypoint /usr/local/bin/entrypoint

RUN chmod +x /usr/local/bin/entrypoint
RUN chmod +x /etc/cron.d/schedule

CMD php-fpm
EXPOSE 9000


FROM builder AS build1

WORKDIR /var/www

# CMD php artisan octane:start --server="swoole" --host=0.0.0.0 --rpc-port=6001 --port=8000
ENTRYPOINT [ "/usr/local/bin/entrypoint" ]
EXPOSE 8000
