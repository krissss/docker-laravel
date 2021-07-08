# from

参考 [laravel/sail](https://github.com/laravel/sail) 的 Dockerfile

# build

```bash
# 8.0
cd 8.0
docker build -t laravel-sail-docker-8.0 .
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

问题：build 后使用报错：`/usr/bin/env: 'bash\r': No such file or directory`

解决：将 `start-container` 的文件格式从 CRLF 改成 LF
