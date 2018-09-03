FROM php:7-apache

MAINTAINER Rindevich version: 0.1

RUN apt-get update -qq && apt-get install -y -qq apt-utils && mkdir -p /usr/share/man/man1 \
    && apt-get update -qq && apt-get install -y -qq openjdk-8-jre-headless \
    && apt-get update -qq && apt-get install -y -qq  openjdk-8-jdk && dpkg --configure -a

RUN apt-get update -qq && apt-get install -y -qq \
        libicu-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libcurl4-openssl-dev \
        software-properties-common  \
        libcurl3 curl \
        zip \
        unzip \
        inotify-tools \
        build-essential \
        libxml2-dev libxslt1-dev zlib1g-dev \
        git \
        mc \
        htop \
        mysql-client \
        sshpass \
        gnupg \
        nano \
        sudo \
        graphviz \
        netcat-openbsd \
        ant \
        libmagickwand-dev \
        imagemagick \
        libicu-dev

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - && apt-get install -y nodejs build-essential

RUN docker-php-ext-install iconv mbstring opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install curl \
    && docker-php-ext-install intl \
    && docker-php-ext-install pdo \
    && docker-php-ext-install pdo_mysql

RUN pecl install mcrypt-1.0.1

RUN pecl install apcu
RUN echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini

#RUN pecl install memcached
#RUN echo "extension=memcached.so" > /usr/local/etc/php/conf.d/memcached.ini

RUN pecl install imagick
RUN echo "extension=imagick.so" > /usr/local/etc/php/conf.d/imagick.ini

RUN mkdir /tmp/ioncube && \
    mkdir -p /usr/local/ioncube && \
    cd /tmp/ioncube && \
    curl -SL http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz -o /tmp/ioncube/ioncube_loaders_lin_x86-64.tar.gz && \
    tar xvf /tmp/ioncube/ioncube_loaders_lin_x86-64.tar.gz && \
    cd `php -i | grep extension_dir | cut -d' ' -f 5` && \
    cp /tmp/ioncube/ioncube/ioncube_loader_lin_7.2.so /usr/local/ioncube/ioncube_loader_lin_7.2.so && \
    echo zend_extension=/usr/local/ioncube/ioncube_loader_lin_7.2.so > /usr/local/etc/php/conf.d/00-ioncube.ini && \
    rm -rf /tmp/ioncube/

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN pecl install redis \
    && docker-php-ext-enable redis

RUN npm install -g grunt-cli bower grunt jasmine

RUN echo "date.timezone=Europe/Berlin" >> /usr/local/etc/php/conf.d/timezone.ini

RUN set -eux; \
	{ \
		echo 'xdebug.remote_enable=1'; \
		echo 'xdebug.remote_handler=dbgp'; \
		echo 'xdebug.remote_host=172.18.0.1'; \
		echo 'xdebug.remote_port=9000'; \
		echo 'xdebug.remote_autostart=0'; \
		echo 'xdebug.remote_connect_back=1'; \
		echo 'xdebug.profiler_output_dir="/var/www/shopware/vendor/shopware/shopware/build/artifacts"'; \
	} > /usr/local/etc/php/conf.d/xdebug.ini

# Apache + PHP requires preforking Apache for best results
RUN a2enmod rewrite && a2dismod mpm_event && a2enmod mpm_prefork
RUN service apache2 restart

RUN echo "alias ll='ls -ahl'" >> /etc/bash.bashrc

WORKDIR /var/www/shopware

RUN apt-get clean && rm -rf /var/lib/apt/lists/*
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/usr/local/bin/ --filename=composer
RUN php -r "unlink('composer-setup.php');"
