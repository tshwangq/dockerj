FROM php:7.3-fpm

MAINTAINER Qun Wang <tshwangq@gmail.com>
COPY . .
COPY docker-php-ext-get /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-php-ext-get

RUN cat  /etc/apt/sources.list
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && mv sources.list /etc/apt/
RUN cat  /etc/apt/sources.list
#RUN echo "Asia/Shanghai" > /etc/timezone
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata

RUN apt-get clean && apt-get update -y
RUN apt-get install -y \
    libmcrypt-dev \
    libicu-dev \
    libpq-dev \
    libbz2-dev \
    libzip-dev \
    zip \
    git \
    unzip \
    curl \
    mc \
    wget \
    libevent-dev \
    librabbitmq-dev
RUN docker-php-ext-install iconv \
    #&& docker-php-ext-install mcrypt \
    && docker-php-ext-install bz2 \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install intl \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip \
    && docker-php-ext-install pgsql pdo pdo_pgsql \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install opcache \
    && docker-php-ext-enable opcache


ENV EXT_REDIS_VERSION=4.3.0 EXT_IGBINARY_VERSION=3.0.1

RUN docker-php-source extract \
    # igbinary
    && mkdir -p /usr/src/php/ext/igbinary \
    &&  curl -fsSL https://github.com/igbinary/igbinary/archive/$EXT_IGBINARY_VERSION.tar.gz | tar xvz -C /usr/src/php/ext/igbinary --strip 1 \
    && docker-php-ext-install igbinary \
    # redis
    && mkdir -p /usr/src/php/ext/redis \
    && curl -fsSL https://github.com/phpredis/phpredis/archive/$EXT_REDIS_VERSION.tar.gz | tar xvz -C /usr/src/php/ext/redis --strip 1 \
    && docker-php-ext-configure redis --enable-redis-igbinary \
    && docker-php-ext-install redis \
    # cleanup
    && docker-php-source delete

RUN pecl channel-update pecl.php.net && \
    pecl install apcu igbinary && \
    # compile Redis with igbinary support
    pecl bundle redis && cd redis && phpize && ./configure --enable-redis-igbinary && make && make install && \
    docker-php-ext-enable redis
# Install GD
RUN apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev
RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/freetype2 \
    --with-png-dir=/usr/include \
    --with-jpeg-dir=/usr/include \
    && docker-php-ext-install gd \
    && docker-php-ext-enable gd

RUN cp php.ini /usr/local/etc/php/
RUN cp ixed.7.1.lin  /usr/local/lib/php/extensions/no-debug-non-zts-20160303
RUN usermod -u 1000 www-data

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
