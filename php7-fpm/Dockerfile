# Pull base image
FROM php:7-fpm

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

RUN useradd -ms /bin/bash www \
&& usermod -aG root www \
&& chown www:www /home/www \
&& chmod a+rwx /home/www

RUN apt-get update \
&& apt-get install --no-install-recommends --no-upgrade -y \
ssh-client git wget

# ------------------ oracle jdk ------------------

RUN wget -O /tmp/jdk.tar.gz -nv --no-check-certificate -c --header "Cookie: oraclelicense=accept-securebackup-cookie" https://download.oracle.com/otn-pub/java/jdk/8u191-b12/2787e4a523244c269598db4e85c51e0c/jdk-8u191-linux-x64.tar.gz

RUN mkdir /opt/oracle \
&& tar zxvf /tmp/jdk.tar.gz -C /opt/oracle \
&& mv /opt/oracle/jdk* /opt/oracle/jdk \
&& rm -rf /opt/oracle/jdk1* /opt/oracle/jdk/*.zip

ENV JAVA_HOME /opt/oracle/jdk
ENV PATH $PATH:$JAVA_HOME/bin

# ------------------ oracle jdk ------------------

## enabling ext bcmath
RUN docker-php-ext-install bcmath

## enabling ext mysqli
RUN docker-php-ext-install pdo_mysql mysqli

## enabling ext zip
RUN apt-get install --no-install-recommends --no-upgrade -y libzip-dev \
&& pecl install zip \
&& docker-php-ext-enable zip

## enabling ext memcached
RUN apt-get install --no-install-recommends --no-upgrade -y libmemcached-dev \
&& pecl install memcached \
&& docker-php-ext-enable memcached

## enabling ext pgsql
RUN apt-get install --no-install-recommends --no-upgrade -y libpq-dev \
&& docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql \
&& docker-php-ext-install pdo_pgsql pgsql

## enabling ext apcu
RUN pecl install apcu \
&& docker-php-ext-enable apcu

RUN apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN cd ~ \
&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('SHA384', 'composer-setup.php') === '93b54496392c062774670ac18b134c3b3a95e5a5e5c8f1a9f115f203b75bf9a129d5daa8ba6a13e2cc8a1da0806388a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
&& php -r "unlink('composer-setup.php');"
