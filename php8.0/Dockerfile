# syntax=docker/dockerfile:experimental
FROM php:8.0-cli
RUN apt-get update  \
        && apt-get install -y -qq --no-install-recommends wget gnupg2 apt-transport-https lsb-release ca-certificates software-properties-common  \
        && wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg \
        && rm /etc/apt/preferences.d/no-debian-php \
        && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list \
        && ls -l /etc/apt/* \
        && apt-get update  \
        && apt-get install -y -qq --no-install-recommends git openssh-client apt-transport-https lsb-release ca-certificates groff jq curl build-essential nodejs software-properties-common php8.0-intl php8.0-gd git php8.0-cli php8.0-curl php8.0-pgsql php8.0-ldap php8.0-sqlite3 php8.0-mysql php8.0-zip php8.0-xml php8.0-mbstring php8.0-dev make libmagickcore-6.q16-2-extra unzip php8.0-redis php8.0-imagick php8.0-dev php8.0-bcmath php8.0-mongodb libsystemd-dev python python-pip python-dev python3 python3-pip python3-dev libpng-dev python-setuptools python3-setuptools  \
        && apt-get autoremove -y  \
        && apt-get autoclean  \
        && apt-get clean  \
        && curl -sL https://deb.nodesource.com/setup_14.x | bash -  \
        && apt-get update  \
        && apt-get install -y nodejs  \
        && rm -rf /tmp/* /var/tmp/* /var/lib/apt/lists/*
RUN cd /tmp/  \
        && wget https://github.com/nikic/php-ast/archive/master.zip  \
        && unzip master.zip
RUN cd /tmp/php-ast-master/  \
        && phpize  \
        && ./configure  \
        && make  \
        && make install  \
        &&rm -rf /tmp/php-ast-master/
RUN echo "extension=ast.so" >> /etc/php/8.0/cli/conf.d/20-ast.ini
RUN cd /tmp  \
        && wget -O php-systemd-src.zip https://github.com/systemd/php-systemd/archive/master.zip  \
        && unzip php-systemd-src.zip  \
        && cd /tmp/php-systemd-master  \
        && phpize  \
        && ./configure --with-systemd  \
        && make  \
        && make install  \
        && rm -rf /tmp/php-systemd-master  \
        && echo "extension=systemd.so" >> /etc/php/8.0/mods-available/systemd.ini
RUN phpenmod zip intl gd systemd
RUN curl -O -L https://phar.phpunit.de/phpunit-9.5.4.phar  \
        && chmod +x phpunit-9.5.4.phar  \
        && mv phpunit-9.5.4.phar /usr/local/bin/phpunit
RUN curl -O -L https://getcomposer.org/composer-stable.phar  \
        && chmod +x composer-stable.phar  \
        && mv composer-stable.phar /usr/local/bin/composer
RUN phpdismod xdebug
RUN npm i -g npm
RUN pip install --upgrade pip
RUN pip install --user --upgrade awsebcli awscli cloudflare
ENV PATH=~/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
RUN echo $PATH