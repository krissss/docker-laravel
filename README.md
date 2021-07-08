# from

参考 [laravel/sail](https://github.com/laravel/sail) 的 Dockerfile

docker hub 地址 [krisss/laravel-sail-docker-8.0](https://hub.docker.com/repository/docker/krisss/laravel-sail-docker-8.0)

# build

```bash
# 8.0
cd 8.0
docker build -t krisss/laravel-sail-docker-8.0 .
```

# docker-compose

```yml
version: '3'
services:
    laravel.test:
        image: laravel-sail-docker-8.0
        ports:
            - '${APP_PORT:-80}:80'
        environment:
            WWWUSER: '${WWWUSER}'
            LARAVEL_SAIL: 1
        volumes:
            - '.:/var/www/html'
            #- './docker/supervisord.conf:/etc/supervisor/conf.d/supervisord.conf'
        networks:
            - sail
networks:
    sail:
        driver: bridge
```

# 注意

- 问题：build 后使用报错：`/usr/bin/env: 'bash\r': No such file or directory`

解决：<del>将 `start-container` 的文件格式从 CRLF 改成 LF</del>（已经在 Dockerfile 中处理）

- 需求：如何支持 laravel/octane 的启动

方案：正常安装 laravel/octane 后，复制一份容器内部的 `/etc/supervisor/conf.d/supervisord.conf` 到本地项目下，如 `docker/supervisord.conf`，然后修改其中的 `command` 命令，然后挂载到 docker-compose.yml 的 volumes 下。

**此方法适用于任何修改 supervisor 的需求，或者挂载其他文件的，比如 php.ini**

- 问题：laravel/octane 不能 watch 变化

解决：在 docker-compose.yml 的 environment 中增加 `CHOKIDAR_USEPOLLING: 'true'`，参考：[issue](https://github.com/laravel/octane/issues/237)
