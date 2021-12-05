ARG ECHO_NODE_VERSION=latest
FROM node:$ECHO_NODE_VERSION

WORKDIR /opt/laravel-echo-server

COPY ./entrypoint.sh ./entrypoint.sh
COPY ./laravel-echo-server.json ./laravel-echo-server.json

RUN \
    npm install -g laravel-echo-server && \
    chmod +x entrypoint.sh

EXPOSE 6001

ENV \
    ECHO_PORT=6001 \
    ECHO_DATABASE=redis \
    ECHO_SERVER_AUTH_HOST=http://localhost \
    ECHO_SERVER_DEBUG=false \
    ECHO_SSL_CERT_PATH= \
    ECHO_SSL_KEY_PATH= \
    ECHO_SSL_CHAIN_PATH= \
    ECHO_SSL_PASSPHRASE= \
    ECHO_PROTOCOL=http \
    ECHO_REDIS_PORT=6379 \
    ECHO_REDIS_HOSTNAME=redis \
    ECHO_DEVMODE=false \
    ECHO_ALLOW_CORS=true \
    ECHO_ALLOW_ORIGIN=http://localhost:80 \
    ECHO_ALLOW_METHODS="GET, POST" \
    ECHO_ALLOW_HEADERS="Origin, Content-Type, X-Auth-Token, X-Requested-With, Accept, Authorization, X-CSRF-TOKEN, X-Socket-Id"

ENTRYPOINT ["./entrypoint.sh"]
CMD ["laravel-echo-server", "start"]
