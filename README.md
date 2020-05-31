# 如何使用docker来构建php项目
## 先决条件

1. Docker

2. Docker-compose

   ```shell
   #也可以使用本项目提供的docker-compose下载好的文件
   sudo cp ./docker-compose-Linux-x86_64 /usr/local/bin/
   cd /usr/local/bin
   sudo mv docker-compose-Linux-x86_64 docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   docker-compose --version
   ```

## 安装

1. 进入项目目录 docker-compose up
2. 如果项目能正常启动,在使用ctrl+c结束
3. 以后台的方式来运行 docker-compose up -d

**注意**：

可能出现文件权限的问题,请自己根据需要修改项目目录的权限

## 扩展安装方式

**由于php默认提供了几个命令工具**

```shell
docker-php-source
docker-php-ext-install
docker-php-ext-enable
docker-php-ext-configure
```

进入php-fpm换掉源

```
echo "deb http://mirrors.163.com/debian/ stretch main non-free contrib" >/etc/apt/sources.list

echo "deb http://mirrors.163.com/debian/ stretch-proposed-updates main non-free contrib" >>/etc/apt/sources.list

echo "deb-src http://mirrors.163.com/debian/ stretch main non-free contrib" >>/etc/apt/sources.list

echo "deb-src http://mirrors.163.com/debian/ stretch-proposed-updates main non-free contrib" >>/etc/apt/sources.list

echo "deb-src http://mirrors.ustc.edu.cn/debian/ stable main contrib non-free" >>/etc/apt/sources.list

echo "deb-src http://mirrors.ustc.edu.cn/debian/ stable-updates main contrib non-free" >>/etc/apt/sources.list
```

例如：

```shell
apt update && apt upgrade -y

# 安装pod_mysql
docker-php-ext-install pdo_mysql

# 安装redis
## https://pecl.php.net/package/redis
php -m | grep redis
pecl install https://pecl.php.net/get/redis-5.0.0.tgz
docker-php-ext-enable redis

# bcmath
docker-php-ext-install bcmath

# 安装gd
apt install zlib1g
apt install zlib1g-dev
apt install libz-dev
apt install -y libfreetype6-dev libjpeg62-turbo-dev libmcrypt-dev libpng-dev 
docker-php-ext-install iconv
apt install libtinfo5
apt install libreadline7   
apt install libtinfo-dev
apt install libmcrypt-dev libreadline-dev
pecl install https://pecl.php.net/get/mcrypt-1.0.3.tgz 
docker-php-ext-enable mcrypt
pecl install https://pecl.php.net/get/gdchart-0.2.0.tgz

apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir --with-jpeg-dir --with-png-dir \
    && docker-php-ext-install -j$(nproc) gd

# 安装session
docker-php-ext-install session

# 安装composer
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer

# 每次安装扩展需要重启服务器
docker restart zler-php-fpm
```

