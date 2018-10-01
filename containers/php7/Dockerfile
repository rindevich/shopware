ARG PHP_VER=7.0

FROM php:${PHP_VER}-apache

ARG PHP_VER

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

RUN docker-php-ext-install iconv opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install curl \
    && docker-php-ext-install intl \
    && docker-php-ext-install pdo \
    && docker-php-ext-install pdo_mysql

RUN { \
        echo 'opcache.memory_consumption=128'; \
        echo 'opcache.interned_strings_buffer=8'; \
        echo 'opcache.max_accelerated_files=4000'; \
        echo 'opcache.revalidate_freq=60'; \
        echo 'opcache.fast_shutdown=1'; \
        echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini

RUN { \
        echo 'phar.readonly = 0'; \
        echo 'short_open_tag = Off'; \
        echo 'max_execution_time = 0'; \
        echo 'memory_limit = 512M'; \
        echo 'upload_max_filesize = 100M'; \
        echo 'post_max_size = 100M'; \
    } > /usr/local/etc/php/conf.d/php-config.ini

RUN set -xe; \
    if [ "${PHP_VER}" = "7.2" ]; then \
        pecl install mcrypt-1.0.1; \
        docker-php-ext-enable mcrypt; \
    else \
        docker-php-ext-install mcrypt; \
    fi

RUN pecl install apcu
RUN echo "extension=apcu.so" > /usr/local/etc/php/conf.d/apcu.ini

RUN pecl install imagick
RUN echo "extension=imagick.so" > /usr/local/etc/php/conf.d/imagick.ini

ADD https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz /tmp/

RUN tar xvzfC /tmp/ioncube_loaders_lin_x86-64.tar.gz /tmp/ && \
	php_ext_dir="$(php -i | grep extension_dir | head -n1 | awk '{print $3}')" && \
	mv /tmp/ioncube/ioncube_loader_lin_${PHP_VER}.so "${php_ext_dir}/" && \
    echo "zend_extension = $php_ext_dir/ioncube_loader_lin_${PHP_VER}.so" \
        > /usr/local/etc/php/conf.d/00-ioncube.ini && \
	rm /tmp/ioncube_loaders_lin_x86-64.tar.gz && \
	rm -rf /tmp/ioncube

RUN pecl install xdebug && docker-php-ext-enable xdebug
RUN pecl install redis && docker-php-ext-enable redis
RUN npm install -g grunt-cli bower grunt jasmine
RUN echo "date.timezone=Europe/Berlin" >> /usr/local/etc/php/conf.d/timezone.ini

RUN set -eux; \
	{ \
		echo 'xdebug.remote_enable=1'; \
		echo 'xdebug.remote_handler=dbgp'; \
		echo 'xdebug.remote_host=127.0.0.1'; \
		echo 'xdebug.remote_port=9000'; \
		echo 'xdebug.remote_autostart=0'; \
		echo 'xdebug.remote_connect_back=1'; \
		echo 'xdebug.max_nesting_level=500'; \
		echo 'xdebug.profiler_output_dir="/var/www/shopware/vendor/shopware/shopware/build/artifacts"'; \
	} > /usr/local/etc/php/conf.d/xdebug.ini

RUN a2enmod rewrite && a2dismod mpm_event && a2enmod mpm_prefork
RUN service apache2 restart

RUN echo "alias ll='ls -ahl'" >> /etc/bash.bashrc

RUN apt-get clean && rm -rf /var/lib/apt/lists/*
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/usr/local/bin/ --filename=composer
RUN php -r "unlink('composer-setup.php');"

WORKDIR /var/www/shopware