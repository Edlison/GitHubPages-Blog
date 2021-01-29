# 使用Docker搭建图床

安装容器编排工具Compose  
`pip3 install docker-compose`

pull镜像  
`docker pull mariadb:latest`  
`docker pull nmtan/chevereto:latest`  

使用Docker-Compose启动  
`mkdir cheverto/`  
`cd cheverto`  
`touch docker-compose.yaml`

docker-compose.yaml配置文件

```yml
version: '3'

services:
  db:
    image: mariadb
    volumes:
      - database:/var/lib/mysql:rw
    restart: always
    networks:
      - private
    environment:
      MYSQL_ROOT_PASSWORD: chevereto_root
      MYSQL_DATABASE: chevereto
      MYSQL_USER: chevereto
      MYSQL_PASSWORD: chevereto

  chevereto:
    depends_on:
      - db
    image: nmtan/chevereto
    restart: always
    networks:
      - private
    environment:
      CHEVERETO_DB_HOST: db
      CHEVERETO_DB_USERNAME: chevereto
      CHEVERETO_DB_PASSWORD: chevereto
      CHEVERETO_DB_NAME: chevereto
      CHEVERETO_DB_PREFIX: chv_
    volumes:
      - chevereto_images:/var/www/html/images:rw
      - chevereto_php_config:/usr/local/etc/php:rw
    ports:
      - 12000:80

networks:
  private:
volumes:
  database:
  chevereto_images:
  chevereto_php_config:
```

对应挂载到当前文件夹中的php.ini文件(**注意** php.ini文件需要先创建 否则docker会自动创建同名文件夹)
`- ./php.ini:/usr/local/etc/php/php.ini:rw`

php.ini
```ini
memory_limit = 256M;
upload_max_filesize = 100M;
post_max_size = 100M;
```

运行Docker-Compose  
`docker-compose up -d`