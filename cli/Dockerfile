FROM php:5.6-alpine
RUN apk add -U --no-cache autoconf alpine-sdk mariadb-dev \
      libmemcached-dev \
      libxml2-dev \
      libxslt-dev \
      freetype-dev \
      libjpeg-turbo-dev \
      libmcrypt-dev \
      libpng-dev
RUN apk add -U --no-cache pcre-dev
RUN pecl install apcu-4.0.11
RUN docker-php-ext-enable apcu
RUN apk add -U --no-cache cyrus-sasl-dev
RUN pecl install memcached-2.2.0
RUN docker-php-ext-enable memcached
RUN pecl install memcache-2.2.7
RUN docker-php-ext-enable memcache
RUN docker-php-ext-install mysql
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install soap
RUN docker-php-ext-install xsl
RUN docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr/include
RUN docker-php-ext-install gd mbstring pdo pdo_mysql zip
RUN apk del --purge autoconf alpine-sdk

