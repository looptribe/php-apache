FROM ubuntu:16.04

MAINTAINER Diego Ferri <diego.ferri@looptribe.com>

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -o Dpkg::Options::="--force-confnew" install -y \
    software-properties-common

RUN apt-add-repository -y ppa:ondrej/php && apt-get update

RUN apt-get update &&  DEBIAN_FRONTEND=noninteractive apt-get install -y --allow-unauthenticated \
	libapache2-mod-php5.6 \
	apache2 \
	php5.6 \
	php5.6-curl \
	php5.6-dev \
	php5.6-gd \
	php5.6-intl \
	php5.6-mcrypt \
	php5.6-mysql \
	php5.6-sqlite3 \
	php5.6-xml \
	php-xdebug \
	git \
	zip \
	curl

RUN a2enmod expires && \
	a2enmod rewrite

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

COPY web.ini /etc/php/5.6/apache2/conf.d/90-web.ini
COPY web.ini /etc/php/5.6/cli/conf.d/90-web.ini

RUN rm /etc/apache2/sites-enabled/*
COPY vhost.conf /etc/apache2/sites-available/app.conf
RUN a2ensite app

RUN curl -sS https://getcomposer.org/installer | \
	php -- --install-dir=/usr/local/bin

COPY apache2-foreground /usr/local/bin/

RUN usermod -u 1000 www-data
RUN mkdir -p /var/www/app && chown -R www-data:www-data /var/www/app

WORKDIR /var/www/app
VOLUME /var/www/app

EXPOSE 80
EXPOSE 443

CMD ["apache2-foreground"]