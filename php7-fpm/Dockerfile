# Pull base image
FROM php:7-fpm

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

RUN useradd -ms /bin/bash www \
&& usermod -aG root www \
&& chown www:www /home/www \
&& chmod a+rwx /home/www \
&& mkdir -p /usr/share/man/man1 \
&& apt-get update \
&& apt-get install --no-install-recommends --no-upgrade -y ssh-client git wget libzip-dev libmemcached-dev libicu-dev libpq-dev zip unzip default-jre \
&& docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql \
&& docker-php-ext-install bcmath pdo_mysql mysqli opcache intl pdo_pgsql pgsql \
&& pecl install redis \
&& pecl install xdebug \
&& docker-php-ext-configure zip --with-libzip \
&& pecl install zip \
&& pecl install memcached \
&& pecl install apcu \
&& docker-php-ext-enable redis xdebug zip memcached apcu \
&& apt-get autoremove && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
&& cd ~ \
&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
&& php -r "unlink('composer-setup.php');"