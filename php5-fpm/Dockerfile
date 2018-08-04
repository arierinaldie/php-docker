# https://github.com/rinaldiarie/php-docker/tree/master/php5-fpm

# Pull base image
FROM phpdockerio/php56-fpm:latest

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y --no-install-recommends install \
        php5-memcached \
        php5-mongo \
        php5-mysql \
        php5-pgsql \
        php5-redis \
        php5-sqlite \
        php5-adodb \
        php5-gd \
        php5-imap \
        php5-imagick \
        php5-intl \
        php5-memcache \
        php5-msgpack \
        php5-oauth \
        php5-odbc \
        php5-phpdbg \
        php5-ssh2 \
        php5-xdebug \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Installing java using Oracle JDK/JRE Installer
RUN apt-get update \
	&& apt-get -y --no-install-recommends install \
		software-properties-common \
		ssh-client

RUN add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 boolean true | /usr/bin/debconf-set-selections \
	&& apt-get update \
	&& apt-get install -y oracle-java8-set-default \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/cache/oracle-jdk8-installer

# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle