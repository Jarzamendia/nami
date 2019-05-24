FROM php:7.3-fpm
# Install Dependencies
RUN apt-get update && apt-get install -y locales unixodbc libgss3 odbcinst \
    devscripts debhelper dh-exec dh-autoreconf libreadline-dev libltdl-dev \
    unixodbc-dev wget unzip \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install pdo \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen

# Add Microsoft repo for Microsoft ODBC Driver 17 for Linux
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install -y apt-transport-https
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install -y msodbcsql17

# Enable the php extensions.
RUN pecl install pdo_sqlsrv-5.6.1 sqlsrv-5.6.1 \
    && docker-php-ext-enable pdo_sqlsrv sqlsrv

ENV NAMIVersion="2.2.1"

RUN curl -O https://codeload.github.com/ProjectNami/projectnami/tar.gz/${NAMIVersion} \
&& gzip -dc ${NAMIVersion} | tar -xf - -C /tmp \
&& mv /tmp/projectnami-${NAMIVersion}/* /var/www/html \
&& rm -rf ${NAMIVersion}; rm -rf /tmp/projectnami-${NAMIVersion} \
&& chown -R www-data:www-data /var/www/html

VOLUME [ "/var/www/html/" ]

ENTRYPOINT ["apache2-foreground"]