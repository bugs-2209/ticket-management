#Use an offical PHP --version 8.1 image with Apache as the base image
FROM php:8.2-apache

# Set enviroment variables
ENV ACCEPT_EULA = Y
ENV COMPOSER_ALLOW_SUPERUSER=1
LABEL maintainer="cuogg.contact@gmail.com"

# Install system dependencies.
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    zip \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# Enable Apache modules required for Laravel.
RUN a2enmod rewrite

# Set the Apache document root
ENV APACHE_DOCUMENT_ROOT /var/www/html/public

# Update the default Apache site configuration
COPY ../docker/apache/apache-config.conf /etc/apache2/sites-available/000-default.conf

# Install PHP extensions.
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd pdo pdo_mysql

# Install Composer globally.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Create a directory for your Laravel application.
WORKDIR /var/www/html

# Copy the Laravel application files into the container.
COPY ../ .
RUN chown -R www-data:www-data /var/www

# Install Laravel dependencies using Composer.
RUN composer install --no-interaction --optimize-autoloader

# Set permissions for Laravel.
RUN chown -R www-data:www-data storage bootstrap/cache

# Expose port 80 for Apache.
EXPOSE 80

# Start Apache web server.
CMD ["apache2-foreground"]

# Can add any additional configurations or commands required for Laravel 10 here.
