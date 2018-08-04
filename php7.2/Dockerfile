#
# Xamarie php7.2-fpm Dockerfile
#
# https://bitbucket.org/arierinaldie/phpdocker
#

# Pull base image
FROM phpdockerio/php72-fpm:latest

WORKDIR "/application"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y --no-install-recommends install \
		php-memcached \
		php-mongodb \
		php7.2-mysql \
		php7.2-pgsql \
		php-redis \
		php7.2-sqlite3 \
		php7.2-gd \
		php7.2-imap \
		php-imagick \
		php-msgpack \
		php7.2-odbc \
		php7.2-phpdbg \
		php-ssh2 \
		php-xdebug \
		php7.2-tidy \
		php-yaml \
		php-zmq \
		curl \
		unzip \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN cd ~
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"
	
# Installing java using Oracle JDK/JRE Installer
RUN apt-get update \
	&& apt-get -y --no-install-recommends install \
		software-properties-common \
		ssh-client

# Installing java using Default JDK/JRE
RUN apt-get update \
	&& apt-get -y --no-install-recommends install \
		default-jre \
	&& apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /var/cache/*
