#
# Phodus
#

FROM debian:jessie

# disable interactive functions
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -q
RUN apt-get install apache2 wget curl -yq

RUN apt-get install -yq php5=5.6.7+dfsg-1 php5-cli php5-curl
RUN apt-get install -y make php-pear php5-dev git

# Install Composer
RUN curl -s https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# let's copy a few of the settings from /etc/init.d/apache2
ENV APACHE_CONFDIR /etc/apache2
ENV APACHE_ENVVARS $APACHE_CONFDIR/envvars
# and then a few more from $APACHE_CONFDIR/envvars itself
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_PID_FILE $APACHE_RUN_DIR/apache2.pid
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_LOG_DIR /var/log/apache2

RUN sed -i 's/\;date\.timezone\ \=/date\.timezone\ \=\ "UTC"/g' /etc/php5/cli/php.ini
RUN sed -i 's/\;date\.timezone\ \=/date\.timezone\ \=\ "UTC"/g' /etc/php5/apache2/php.ini
RUN a2enmod php5 rewrite
RUN a2enmod rewrite
RUN a2enmod headers

RUN echo "servername phodus_dev" >> /etc/apache2/apache2.conf

ENV HOSTNAME phodus_dev
ENV APPLICATION_ENV dev

VOLUME ["/var/log/apache2","/etc/apache2/sites-enabled", "/var/www"]

EXPOSE 80

# Start services
CMD ["apache2", "-DFOREGROUND"]
