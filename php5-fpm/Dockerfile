# Pull base image
FROM php:5-fpm

RUN useradd -ms /bin/bash www \
&& usermod -aG root www \
&& chown www:www /home/www \
&& chmod a+rwx /home/www \
&& mkdir -p /usr/share/man/man1 \
&& apt-get install --no-install-recommends --no-upgrade -y software-properties-common dirmngr ssh-client unzip git default-jre libmemcached-dev zlib1g-dev libpq-dev libicu-dev libpng-dev libjpeg-dev libmagickwand-dev libfreetype6-dev \
&& docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
&& docker-php-ext-install pdo pdo_pgsql pgsql pdo_mysql mysqli intl \
&& docker-php-ext-install -j$(nproc) iconv \
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
&& cd ~ \
&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
&& php -r "unlink('composer-setup.php');"