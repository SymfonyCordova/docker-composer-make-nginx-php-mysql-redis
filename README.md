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

## 扩展安装方式

**由于php默认提供了几个命令工具**

```shell
docker-php-source
docker-php-ext-install
docker-php-ext-enable
docker-php-ext-configure
```

