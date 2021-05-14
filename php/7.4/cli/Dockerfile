FROM php:7.4-cli-alpine

# 设置时区为上海
RUN apk add --no-cache tzdata && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && apk del tzdata

# 安装一些依赖和（我认为）必要的组件
RUN apk --no-cache add --virtual .build_deps gcc g++ make autoconf \
    && docker-php-ext-install bcmath pdo_mysql pcntl opcache \
    && pecl install -o redis swoole \
    && docker-php-ext-enable swoole redis \
    && apk del .build_deps

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

STOPSIGNAL SIGQUIT

COPY entrypoint /usr/local/bin/

RUN chmod +x /usr/local/bin/entrypoint

ENTRYPOINT [ "entrypoint" ]

CMD [ "php", "-a" ]