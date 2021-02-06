FROM debian:buster-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN echo 'APT::Install-Recommends "0" ; APT::Install-Suggests "0" ;' > /etc/apt/apt.conf.d/01-no-recommended && \
    echo 'path-exclude=/usr/share/man/*' > /etc/dpkg/dpkg.cfg.d/path_exclusions && \
    echo 'path-exclude=/usr/share/doc/*' >> /etc/dpkg/dpkg.cfg.d/path_exclusions && \
    apt-get update && \
    apt-get --yes install lsb-release apt-transport-https ca-certificates wget gnupg && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN echo mysql-apt-config mysql-apt-config/select-server select mysql-8.0 | debconf-set-selections && \
    wget -O mysql-apt-config.deb https://dev.mysql.com/get/mysql-apt-config_0.8.13-1_all.deb && \
    dpkg -i mysql-apt-config.deb

RUN apt-get update && \
    apt-get install --yes -o Dpkg::Options::=--force-confnew mysql-server && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    apt-get --yes install imagemagick libmagickcore-6.q16-2-extra \
        ghostscript \
        php7.3-fpm \
        php7.3-cli \
        php7.3-intl \
        php7.3-opcache \
        php7.3-mysql \
        php7.3-zip \
        php7.3-xml \
        php7.3-gd \
        php7.3-curl \
        php7.3-mbstring \
        php7.3-bcmath \
        php7.3-imagick \
        php7.3-apcu \
        php7.3-exif \
        openssh-client \
        php-memcached \
        aspell \
        aspell-en aspell-es aspell-de aspell-fr && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    ln -s /usr/sbin/php-fpm7.3 /usr/local/sbin/php-fpm && \
    usermod --uid 1000 www-data && groupmod --gid 1000 www-data && \
    mkdir /srv/pim && \
    sed -i "s#listen = /run/php/php7.3-fpm.sock#listen = 9000#g" /etc/php/7.3/fpm/pool.d/www.conf && \
    mkdir -p /run/php

COPY build/akeneo.ini /etc/php/7.3/mods-available/akeneo.ini
RUN phpenmod akeneo
