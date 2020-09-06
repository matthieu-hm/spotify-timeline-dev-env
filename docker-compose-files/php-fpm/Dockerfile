FROM php:7.4-fpm
WORKDIR "/application"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Use the default development configuration
RUN cp "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

# Install PHP modules
RUN apt-get update -y && apt-get install -y \
    # GD requirement
    libpng-dev \
    libjpeg-dev \
    libwebp-dev \
    libfreetype6-dev \
    # intl requirement
    libicu-dev \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install bcmath
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install intl

# Install xdebug
RUN pecl install xdebug && docker-php-ext-enable xdebug

# Enable xdebug for CLI when using `docker-compose exec {dockerId} bash`
RUN echo "export XDEBUG_CONFIG=\"idekey=VSCODE\"" >> "/root/.bashrc"

# Install GIT
RUN apt-get update -y && apt-get install -y git \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Install COMPOSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
