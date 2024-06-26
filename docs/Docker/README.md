# Docker 

## Docker Installation

### Ubuntu

[参考](https://cloud.tencent.com/developer/article/2309562)


## Docker Compose

[ref](https://www.freecodecamp.org/chinese/news/what-is-docker-compose-how-to-use-it/)

Docker Compose能够在 Docker 节点上，以单引擎模式(Single-Engine Mode)进行多容器应用的部 署和管理。多数的现代应用通过多个更小的微服务互相协同来组成一个完整可用的应用。

Docker Compose 是一个工具，你可以用来定义和分享多容器应用程序。这意味着你可以使用一个单一的资源来运行一个有多个容器的项目。

例如，假设你正在用 NodeJS 和 MongoDB 一起构建一个项目。你可以创建一个单一的镜像，将两个容器作为一个服务来启动,你不需要分别启动每个容器。

### docker-compose.yml

compose 文件是一个 YML 文件，定义了 Docker 容器的服务、网络和卷。有几个版本的 compose 文件格式可用——1、2、2.x 和 3.x。

```yml

version: '3'
services:
  app:
    image: node:latest
    container_name: app_main
    restart: always
    command: sh -c "yarn install && yarn start"
    ports:
      - 8000:8000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: localhost
      MYSQL_USER: root
      MYSQL_PASSWORD: 
      MYSQL_DB: test
  mongo:
    image: mongo
    container_name: app_mongo
    restart: always
    ports:
      - 27017:27017
    volumes:
      - ~/mongo:/data/db
volumes:
  mongodb:
```

让我们来拆解一下上面的代码，并逐条理解：

- version 指的是 docker-compose 的版本（最新的是 3）
- services 定义了我们需要运行的服务
- app 是你的一个容器的自定义名称
- image 指的是我们要拉取的镜像，这里我们使用的是 node:latest 和 mongo
- container_name 是每个容器的名称
- restart 启动/重启一个服务容器
- port 定义了运行该容器的自定义端口
- working_dir 是服务容器的当前工作目录
- environment 定义了环境变量，如 DB 凭证等
- command 是运行该服务的命令

#### 如何运行多容器

我们需要使用 docker build 来构建我们的多容器。

```docker compose build```

成功构建后，我们可以使用 up 命令运行容器。

`docker compose up`

如果你想在后台运行容器，只需使用 -d 标志（detach）。

`docker compose up -d`

容器运行之后，你可以使用 ps 命令来查看正在运行的容器。

`docker compose ps`