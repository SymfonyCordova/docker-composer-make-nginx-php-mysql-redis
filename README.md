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
pecl install igbinary
docker-php-ext-enable igbinary
pecl install https://pecl.php.net/get/redis-5.0.0.tgz
docker-php-ext-enable redis

# bcmath
docker-php-ext-install bcmath

# 安装gd
# 可能需要安装这些库
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

# 大部分情况下面命令行就可以了
apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir --with-jpeg-dir --with-png-dir \
    && docker-php-ext-install -j$(nproc) gd

# 安装composer
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer

# 每次安装扩展需要重启服务器
docker restart zler-php-fpm
```

## 安装elasticsearch中文插件

```shell
 docker exec -ti zler-elasticsearch bash
 
./bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v5.5.1/elasticsearch-analysis-ik-5.5.1.zip
```



# 如何构建gitlab代码仓库

1. 文档: https://docs.gitlab.com/omnibus/docker/README.html#install-gitlab-using-docker-engine

2. 创建环境变量

   对于Linux用户，请将路径设置为`/srv`：

   ```shell
   export GITLAB_HOME=/srv
   ```

   对于Mac OS用户，请使用用户的`$HOME`文件夹：

   ```shell
   export GITLAB_HOME=$HOME
   ```

3. GitLab容器使用主机安装的卷来存储持久数据：

   | 宿主机位置                   | 容器位置          | 作用                   |
   | ---------------------------- | ----------------- | ---------------------- |
   | `$GITLAB_HOME/gitlab/data`   | `/var/opt/gitlab` | 用于存储应用程序数据   |
   | `$GITLAB_HOME/gitlab/logs`   | `/var/log/gitlab` | 用于存储日志           |
   | `$GITLAB_HOME/gitlab/config` | `/etc/gitlab`     | 用于存储GitLab配置文件 |

4. Docker-compose运行

   ```yaml
   web:
     image: 'gitlab/gitlab-ce:latest'
     restart: always
     hostname: 'gitlab.mostyour.com'
     environment:
       GITLAB_OMNIBUS_CONFIG: |
         external_url 'https://gitlab.mostyour.com'
         # Add any other gitlab.rb configuration here, each on its own line
     ports:
       - '8000:80'
       - '4430:443'
       - '2200:22'
     volumes:
       - '$GITLAB_HOME/gitlab/config:/etc/gitlab'
       - '$GITLAB_HOME/gitlab/logs:/var/log/gitlab'
       - '$GITLAB_HOME/gitlab/data:/var/opt/gitlab'
   ```

5. 运行容器

   ```shell
   docker-compose up -d
   ## 初始化过程可能需要很长时间。您可以使用以下方法跟踪此过程：
   sudo docker logs -f gitlab
   ```








