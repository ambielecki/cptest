FROM ubuntu:20.04
ENV PHP_IDE_CONFIG="serverName=CommDev"

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y software-properties-common
RUN apt install -y apache2 && apt install -y apache2-utils
RUN a2enmod rewrite && a2enmod headers

RUN add-apt-repository ppa:ondrej/php && apt-get update

# install php7.4
RUN apt -y install php7.4

# general dependencies
RUN apt-get install -y \
    curl \
    fontconfig \
    git \
    gnupg \
    libgd-dev \
    libxml2-dev \
    libxrender1 \
    libzip-dev \
    nano \
    unzip \
    vim \
    xfonts-75dpi \
    xfonts-base \
    wget \
    zip \
    zlib1g-dev

# php modules \
RUN apt-get install -y \
    php7.4-curl \
    php7.4-gd \
    php7.4-mbstring \
    php7.4-mysqli \
    php7.4-mysql \
    php7.4-pdo \
    php7.4-soap \
    php7.4-xdebug \
    php7.4-xml \
    php7.4-zip


# Install node and npm
RUN apt-get update -yq \
    && curl -sL https://deb.nodesource.com/setup_20.x | bash \
    && apt-get install nodejs -yq

# Install composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    printf "\nPATH=\"~/.config/composer/vendor/bin:\$PATH\"\n" | tee -a ~/.bashrc

# wkhtlmtopdf
RUN wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb && apt install ./wkhtmltox_0.12.6-1.focal_amd64.deb

RUN mkdir -p /opt/wkhtmltopdf/bin/ && cd /opt/wkhtmltopdf/bin/ && ln -s /usr/local/bin/wkhtmltopdf .

COPY commdev.test.conf /etc/apache2/sites-available/commdev.test.conf
RUN a2ensite commdev.test.conf && a2dissite 000-default.conf

COPY ./my-conf.ini /etc/php/7.4/apache2/conf.d/my-conf.ini
COPY ./xdebug.ini /etc/php/7.4/mods-available/xdebug.ini
RUN cd /var/www && mkdir log && touch log/xdebug.log

RUN composer global require psy/psysh

RUN echo "ServerName 127.0.0.1" >> /etc/apache2/apache2.conf
RUN useradd -ms /bin/bash ambielecki

ENV APACHE_RUN_USER=ambielecki
ENV APACHE_RUN_GROUP=ambielecki

EXPOSE 80
CMD ["apache2ctl", "-D", "FOREGROUND"]