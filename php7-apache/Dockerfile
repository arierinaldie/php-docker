# Pull base image
FROM php:7-apache

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

RUN useradd -ms /bin/bash www \
&& usermod -aG root www \
&& chown www:www /home/www \
&& chmod a+rwx /home/www \
&& mkdir -p /usr/share/man/man1 \
&& apt-get update \
&& apt-get install --no-install-recommends --no-upgrade -y software-properties-common dirmngr ssh-client unzip git default-jre libmemcached-dev libzip-dev libpq-dev libicu-dev libpng-dev libjpeg-dev libmagickwand-dev libfreetype6-dev \
&& docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
&& docker-php-ext-install pdo pdo_pgsql pgsql pdo_mysql mysqli intl \
&& docker-php-ext-install -j$(nproc) iconv \
&& docker-php-ext-configure zip --with-libzip \
&& docker-php-ext-install zip \
&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
&& docker-php-ext-install -j$(nproc) gd \
&& docker-php-ext-install calendar \
&& pecl install memcached-2.2.0 \
&& pecl install apcu-4.0.11 \
&& pecl install redis-2.2.8 \
&& pecl install imagick \
&& pecl install xdebug-2.5.5 \
&& docker-php-ext-enable memcached redis imagick apcu xdebug \
&& apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
&& a2enmod rewrite && service apache2 restart && cd ~ \
&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
&& php -r "unlink('composer-setup.php');"