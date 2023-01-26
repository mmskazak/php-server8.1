# Для начала указываем исходный образ, он будет использован как основа
# RUN выполняет идущую за ней команду в контексте нашего образа.
# В данном случае мы установим некоторые зависимости и модули PHP.
# Для установки модулей используем команду docker-php-ext-install.
# На каждый RUN создается новый слой в образе, поэтому рекомендуется объединять команды.
FROM php:8.1-fpm

# Для создание пользователя в образе
ARG PHP_USER
ARG PHP_USER_PASSWORD

ENV PHP_USER $PHP_USER
ENV PHP_USER_PASSWORD $PHP_USER_PASSWORD

RUN useradd -G root  -p $PHP_USER_PASSWORD -s /bin/bash $PHP_USER
RUN install -d -m 755 -o $PHP_USER -g $PHP_USER /home/$PHP_USER


RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        libonig-dev \
        graphviz \
        unzip \
    && docker-php-ext-configure gd \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-source delete
	
    
# Куда же без composer'а.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#установка mc и добавление пользователя
RUN apt-get -y install mc supervisor

# установка nodejs + npm 
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash - && \
    apt-get install -y nodejs

RUN npm install -g vite    

# Добавим свой php.ini, можем в нем определять свои значения конфига
ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

USER $PHP_USER
# Указываем рабочую директорию для PHP
WORKDIR /var/www

# Запускаем контейнер
# Из документации: The main purpose of a CMD is to provide defaults for an executing container. These defaults can include an executable, 
# or they can omit the executable, in which case you must specify an ENTRYPOINT instruction as well.
CMD ["php-fpm"]
