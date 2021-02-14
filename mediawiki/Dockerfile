FROM ubuntu:18.04

MAINTAINER Nitish Singh <nitish.singh@gmail.com>
LABEL Description="MediaWiki"
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update && apt-get install -yq --no-install-recommends \
    apt-utils \
    curl \
    git \
    apache2 \
    # Install php 7.3
    libapache2-mod-php7.3 \
    php7.3-mbstring \
    php7.3-mysql \
    php7.3-xml \
    mysql-client \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

COPY php.ini /etc/php/7.3/mods-available/
# Configure apache 
RUN a2enmod rewrite expires
RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/servername.conf
RUN a2enconf servername
# Configure vhost for mediawiki
RUN mkdir /var/lib/mediawiki
COPY mediawiki-1.31.0.tar.gz /var/lib/mediawiki
RUN tar -xvzf /var/lib/mediawiki/mediawiki-1.31.0.tar.gz --directory /var/lib/mediawiki
RUN mv /var/lib/mediawiki/mediawiki-*/* /var/lib/mediawiki
RUN ln -s /var/lib/mediawiki /var/www/html/mediawiki
COPY virtualhost.conf /etc/apache2/sites-available/
RUN a2dissite 000-default
RUN a2ensite virtualhost.conf

EXPOSE 80 443

WORKDIR /var/www/html

RUN rm index.html

CMD apachectl -D FOREGROUND 
