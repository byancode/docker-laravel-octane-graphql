FROM composer:latest

WORKDIR /data

RUN composer create-project \
    --ignore-platform-reqs \
    --no-interaction \
    --prefer-dist \
    laravel/laravel .

RUN composer require \
    --prefer-dist \
    --no-interaction \
    --ignore-platform-reqs \
    rebing/graphql-laravel \
    webonyx/graphql-php \
    spiral/roadrunner \
    laravel/octane

ARG OCTANE_SERVER=swoole
RUN php artisan octane:install --no-interaction --server ${OCTANE_SERVER}
RUN php artisan vendor:publish --provider="Rebing\\GraphQL\\GraphQLServiceProvider"

RUN cp -Ra /data/. /app
