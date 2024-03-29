FROM php:7.2-fpm

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Ubuntu软件源选择中国的服务器
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        git \
        curl \
        wget zip unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
		libssl-dev \
        libzip-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pcntl \
        && docker-php-ext-install bcmath \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && docker-php-ext-install exif \
        && docker-php-ext-install posix \
        && rm -r /var/lib/apt/lists/*

# 将预先下载好的拓展包从宿主机拷贝进去
COPY ./pkg/redis.tgz /home/redis.tgz
COPY ./pkg/yaf.tgz /home/yaf.tgz
COPY ./pkg/xlswriter.tgz /home/xlswriter.tgz
COPY ./pkg/mongodb.tgz /home/mongodb.tgz

# 安装 PECL 拓展，这里我们安装的是Redis
RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
RUN pecl install /home/yaf.tgz && echo "extension=yaf.so" > /usr/local/etc/php/conf.d/yaf.ini
RUN pecl install /home/xlswriter.tgz && echo "extension=xlswriter.so" > /usr/local/etc/php/conf.d/xlswriter.ini
RUN pecl install /home/mongodb.tgz && echo "extension=mongodb.so" > /usr/local/etc/php/conf.d/mongodb.ini

# 安装 Swoole
# RUN cd /home \
#     && git clone https://gitee.com/swoole/swoole.git \
#     && cd swoole \
#     && phpize \
#     && ./configure --enable-openssl -with-php-config=/usr/local/bin/php-config \
#     && make \
#     && make install \
#     && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

# 删除扩展包
RUN rm -f /home/redis.tgz
RUN rm -f /home/yaf.tgz
RUN rm -f /home/xlswriter.tgz
RUN rm -f /home/mongodb.tgz

WORKDIR /data

# 写权限
RUN usermod -u 1000 www-data
RUN chown -R www-data:www-data /opt