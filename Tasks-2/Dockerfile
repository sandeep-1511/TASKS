FROM php:7.4-apache

 
RUN pecl install mongodb && docker-php-ext-enable mongodb

 
RUN apt-get update && apt-get install -y git zip unzip

 
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

 

WORKDIR /var/www/html

 
COPY /php-mongodb-crud /var/www/html/

RUN  echo "<Directory /var/www/html>\n   options indexes FollowSymLinks\n   allowOverride None\n   Require all granted\n</Directory>" | tee -a /etc/apache2/apache2.conf


RUN composer install --no-dev

RUN cd /var/www/html && composer update 


RUN chown -R www-data:www-data /var/www/html


RUN chmod -R 755 /var/www/html

 
RUN a2enmod rewrite
 
EXPOSE 80

