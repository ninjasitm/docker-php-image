# syntax=docker/dockerfile:experimental
FROM php:7.4-cli
RUN apt-get update  \
        && apt-get install -y -qq --no-install-recommends wget gnupg2 apt-transport-https lsb-release ca-certificates software-properties-common  \
        && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
        && rm /etc/apt/preferences.d/no-debian-php \
        && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list \
        && ls -l /etc/apt/* \
        && apt-get update  \
        && apt-get install -y -qq --no-install-recommends git openssh-client apt-transport-https lsb-release ca-certificates groff jq curl build-essential nodejs software-properties-common php7.4-intl php7.4-gd php7.4-exif php7.4-json php7.4-readline php7.4-sqlite3 git php7.4-cli php7.4-curl php7.4-pgsql php7.4-ldap php7.4-sqlite3 php7.4-mysql php7.4-zip php7.4-xml php7.4-mbstring php7.4-dev php7.4-gd php7.4-imagick make libmagickcore-6.q16-2-extra zip unzip php7.4-redis php7.4-imagick php7.4-dev php7.4-bcmath php7.4-mongodb libsystemd-dev python python-pip python-dev python3 python3-pip python3-dev libpng-dev python-setuptools python3-setuptools libcurl4-openssl-dev libzip-dev  \
        && apt-get autoremove -y  \
        && apt-get autoclean  \
        && apt-get clean  \
        && curl -sL https://deb.nodesource.com/setup_14.x | bash -  \
        && apt-get update  \
        && apt-get install -y nodejs libsodium-dev \
        && rm -rf /tmp/* /var/tmp/* /var/lib/apt/lists/*
RUN docker-php-ext-install exif gd sodium pcntl curl zip
RUN docker-php-ext-enable exif gd sodium pcntl curl zip
RUN cd /tmp/  \
        && wget https://github.com/nikic/php-ast/archive/master.zip  \
        && unzip master.zip
RUN cd /tmp/php-ast-master/  \
        && phpize  \
        && ./configure  \
        && make  \
        && make install  \
        &&rm -rf /tmp/php-ast-master/
RUN echo "extension=ast.so" >> /etc/php/7.4/cli/conf.d/20-ast.ini
RUN cd /tmp  \
        && wget -O php-systemd-src.zip https://github.com/systemd/php-systemd/archive/master.zip  \
        && unzip php-systemd-src.zip  \
        && cd /tmp/php-systemd-master  \
        && phpize  \
        && ./configure --with-systemd  \
        && make  \
        && make install  \
        && rm -rf /tmp/php-systemd-master  \
        && echo "extension=systemd.so" >> /etc/php/7.4/mods-available/systemd.ini
RUN phpenmod zip intl gd systemd exif
RUN curl -O -L https://phar.phpunit.de/phpunit-9.5.4.phar  \
        && chmod +x phpunit-9.5.4.phar  \
        && mv phpunit-9.5.4.phar /usr/local/bin/phpunit
RUN curl -O -L https://getcomposer.org/composer-stable.phar  \
        && chmod +x composer-stable.phar  \
        && mv composer-stable.phar /usr/local/bin/composer
RUN phpdismod xdebug
RUN npm i -g npm
RUN pip install --upgrade pip
RUN pip install --user --upgrade awsebcli==3.19.4 awscli cloudflare
RUN curl -sSL https://sdk.cloud.google.com | bash
ENV PATH=~/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/root/google-cloud-sdk/bin
RUN echo $PATH