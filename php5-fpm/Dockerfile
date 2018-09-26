# https://github.com/rinaldiarie/php-docker/tree/master/php5-fpm

# Pull base image
FROM php:5-fpm

RUN apt-get update
RUN apt-get -y --no-install-recommends install \
    software-properties-common \
    dirmngr \
    ssh-client

RUN apt-get -y --no-install-recommends install \
    libmemcached-dev \
    zlib1g-dev \
    libpq-dev \
    libicu-dev \
    libpng-dev \
    libjpeg-dev \
    libmagickwand-dev \
    libfreetype6-dev \
&& docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
&& docker-php-ext-install pdo pdo_pgsql pgsql pdo_mysql mysqli intl \
&& docker-php-ext-install -j$(nproc) iconv \
&& docker-php-ext-install zip \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd \
&& pecl install memcached-2.2.0 \
&& pecl install apcu-4.0.11 \
&& pecl install redis-2.2.8 \
&& pecl install imagick \
&& pecl install xdebug-2.5.5 \
&& docker-php-ext-enable memcached redis imagick apcu xdebug

# RUN docker-php-ext-enable memcached redis
    # memcache
    # mongo

# installing default JRE
RUN mkdir -p /usr/share/man/man1
RUN apt-get -y --no-install-recommends install unzip git default-jre

# cleanup
RUN apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# installing composer
RUN cd ~
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"
