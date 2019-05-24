FROM adminer:4.7.1-fastcgi

USER root
RUN apk add --no-cache --virtual .php-ext-deps \
       unixodbc freetds

RUN apk add --no-cache --virtual .build-deps \
        unixodbc-dev freetds-dev

RUN docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr 


RUN docker-php-ext-install \
           pdo_odbc pdo_dblib

RUN  apk del .build-deps \
 && rm -rf /var/cache/apk/*

ADD freetds.conf /etc/freetds.conf

USER adminer
CMD	[ "php", "-S", "[::]:8080", "-t", "/var/www/html" ]
EXPOSE 8080