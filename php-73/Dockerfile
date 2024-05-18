#基于PHP-fpm的镜像  
FROM php:7.3-fpm-bullseye  
  
#设置阿里镜像源  
RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    sed -i 's/security.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list
  
#安装依赖  
RUN apt-get update && \
    apt-get install -y \
        openssl \
        libssl-dev \
        libevent-dev \
        libonig-dev \
        zlib1g-dev \
        libpng-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev  
  
#配置GD扩展编译选项  
RUN docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/freetype2 \
    --with-jpeg-dir=/usr/include  
  
#安装扩展  
RUN docker-php-ext-install -j$(nproc) sockets pcntl bcmath mbstring gd pdo_mysql mysqli  
  
#PECL安装拓展  
RUN pecl install event redis && docker-php-ext-enable redis  
  
#设置工作目录  
WORKDIR /var/www