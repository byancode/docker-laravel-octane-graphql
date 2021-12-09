ARG NODE_VERSION=latest
FROM node:$NODE_VERSION

WORKDIR /opt/laravel-echo-server

COPY ./ssl ./ssl
COPY ./laravel-echo-server.json ./laravel-echo-server.json

RUN npm install -g laravel-echo-server

EXPOSE 6001

ARG ECHO_PORT=6001
ARG ECHO_DEVMODE=false
ARG ECHO_PROTOCOL=http
ARG ECHO_DATABASE=redis
ARG ECHO_DEBUG=false
ARG ECHO_AUTH_HOST=http://localhost
ARG ECHO_SSL_CERT_PATH=/opt/laravel-echo-server/ssl/server.crt
ARG ECHO_SSL_KEY_PATH=/opt/laravel-echo-server/ssl/server.key
ARG ECHO_SSL_CHAIN_PATH=""
ARG ECHO_SSL_PASSPHRASE=""
ARG ECHO_REDIS_PORT=6379
ARG ECHO_REDIS_HOST=redis
ARG ECHO_ALLOW_CORS=true
ARG ECHO_ALLOW_ORIGIN=http://localhost
ARG ECHO_ALLOW_METHODS="GET, POST"
ARG ECHO_ALLOW_HEADERS="Origin, Content-Type, X-Auth-Token, X-Requested-With, Accept, Authorization, X-CSRF-TOKEN, X-Socket-Id"

RUN sed -i -e "s%\"ECHO_PORT\"%$ECHO_PORT%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_DATABASE%$ECHO_DATABASE%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_AUTH_HOST%$ECHO_AUTH_HOST%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_PROTOCOL%$ECHO_PROTOCOL%g" laravel-echo-server.json
RUN sed -i -e "s%\"ECHO_DEVMODE\"%$ECHO_DEVMODE%g" laravel-echo-server.json
RUN sed -i -e "s%\"ECHO_REDIS_PORT\"%$ECHO_REDIS_PORT%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_REDIS_HOST%$ECHO_REDIS_HOST%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_SSL_CERT_PATH%$ECHO_SSL_CERT_PATH%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_SSL_KEY_PATH%$ECHO_SSL_KEY_PATH%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_SSL_CHAIN_PATH%$ECHO_SSL_CHAIN_PATH%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_SSL_PASSPHRASE%$ECHO_SSL_PASSPHRASE%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_ALLOW_CORS%$ECHO_ALLOW_CORS%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_ALLOW_ORIGIN%$ECHO_ALLOW_ORIGIN%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_ALLOW_METHODS%$ECHO_ALLOW_METHODS%g" laravel-echo-server.json
RUN sed -i -e "s%ECHO_ALLOW_HEADERS%$ECHO_ALLOW_HEADERS%g" laravel-echo-server.json

CMD ["laravel-echo-server", "start"]
