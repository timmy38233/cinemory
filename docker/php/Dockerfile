FROM php:8.2.4-fpm-alpine

RUN apk add --update linux-headers

RUN apk update \
    && apk add git \
        curl \
        vim \
        wget \
        bash \
        zlib \
        zlib-dev \
        patch \
        icu-dev

# install php dependencies
RUN apk add --no-cache $PHPIZE_DEPS \
    && pecl install -f xdebug \
    && docker-php-ext-install intl opcache bcmath sockets \
    && docker-php-ext-install pdo pdo_mysql mysqli \
    && docker-php-ext-enable xdebug intl opcache bcmath sockets \
    && rm -rf /var/lib/apt/lists/*

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer selfupdate

# Set timezone
ENV TIMEZONE=Etc/UCT
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
    && "date"

CMD ["php-fpm", "-F"]

COPY . /var/www/cinemory-backend
WORKDIR /var/www/cinemory-backend

EXPOSE 9000
    