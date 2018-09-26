# https://github.com/rinaldiarie/php-docker/tree/master/php7-fpm

# Pull base image
FROM php:7-fpm

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get -y --no-install-recommends install \
    software-properties-common \
    dirmngr \
    ssh-client

RUN useradd -ms /bin/bash webapp
RUN usermod -aG root webapp
RUN chown webapp:webapp /home/webapp
RUN chmod a+rwx /home/webapp

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
