#基于PHP8.1-fpm的镜像
FROM php:7.4-fpm-bullseye

#设置阿里镜像源
RUN sed -i s/deb.debian.org/mirrors.aliyun.com/g /etc/apt/sources.list && \
    sed -i s/security.debian.org/mirrors.aliyun.com/g /etc/apt/sources.list

#安装依赖
RUN apt-get update && \
    apt-get install -y openssl libssl-dev libevent-dev libonig-dev zlib1g-dev libpng-dev libzip-dev \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev && apt-get clean

#安装扩展
RUN docker-php-ext-install sockets pcntl bcmath mbstring zip pdo_mysql mysqli

#安装GD库
RUN docker-php-ext-configure gd --with-freetype --with-jpeg && docker-php-ext-install -j$(nproc) gd

#PECL安装拓展
RUN pecl install event redis && docker-php-ext-enable redis

#这里由于需要先加载sockets再加载event
RUN echo "extension=event" >> /usr/local/etc/php/conf.d/docker-php-ext-sockets.ini

#安装composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#设置工作目录
WORKDIR /var/www
