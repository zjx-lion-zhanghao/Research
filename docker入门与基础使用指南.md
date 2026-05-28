# Docker 入门与基础使用指南

> 本文面向 Docker 初学者，系统介绍 Docker 的基本概念、核心原理、常用命令、Dockerfile、数据卷、网络、Docker Compose 以及常见实践场景。  
> 适合作为课程学习笔记、项目部署说明、GitHub README 或个人技术文档使用。

---

## 目录

- [1. Docker 是什么](#1-docker-是什么)
- [2. 为什么需要 Docker](#2-为什么需要-docker)
- [3. Docker 与虚拟机的区别](#3-docker-与虚拟机的区别)
- [4. Docker 的核心概念](#4-docker-的核心概念)
- [5. 镜像 Image](#5-镜像-image)
- [6. 容器 Container](#6-容器-container)
- [7. Docker 的基本使用流程](#7-docker-的基本使用流程)
- [8. 常用 Docker 命令](#8-常用-docker-命令)
- [9. Dockerfile](#9-dockerfile)
- [10. 使用 Docker 部署一个 Flask 项目](#10-使用-docker-部署一个-flask-项目)
- [11. 数据卷 Volume](#11-数据卷-volume)
- [12. 目录挂载 Bind Mount](#12-目录挂载-bind-mount)
- [13. Docker 网络](#13-docker-网络)
- [14. Docker Compose](#14-docker-compose)
- [15. 常见实用示例](#15-常见实用示例)
- [16. Docker 常见参数解释](#16-docker-常见参数解释)
- [17. Docker 的典型使用场景](#17-docker-的典型使用场景)
- [18. Docker 常见问题与排查](#18-docker-常见问题与排查)
- [19. Docker 学习路线](#19-docker-学习路线)
- [20. 最小必会命令清单](#20-最小必会命令清单)
- [21. 完整练习：用 Docker 启动一个 Nginx 网站](#21-完整练习用-docker-启动一个-nginx-网站)
- [22. 总结](#22-总结)

---

## 1. Docker 是什么

Docker 是一种用于 **打包、分发、运行应用程序** 的容器化工具。

可以简单理解为：

> Docker 可以把一个程序及其运行所需的环境一起打包，使它能够在不同电脑、服务器或云平台上以一致的方式运行。

例如，一个 Python 项目可能依赖：

```text
Python 3.10
Flask 2.3
MySQL 8.0
Ubuntu 系统环境
若干第三方库
```

如果不使用 Docker，其他人在运行这个项目时，需要手动安装对应版本的软件和依赖。  
而使用 Docker 后，可以将这些环境统一打包成镜像，其他人只需要安装 Docker，就可以直接运行。

---

## 2. 为什么需要 Docker

### 2.1 传统开发和部署中的问题

在传统开发中，经常会出现这种情况：

> “明明在我电脑上可以运行，为什么到你电脑上就不行？”

常见原因包括：

```text
Python / Java / Node.js 版本不同
第三方依赖库版本不同
操作系统不同
缺少系统依赖
环境变量配置不同
数据库版本不同
端口配置不同
```

例如你写了一个 Python 项目，在自己电脑上能运行：

```bash
python app.py
```

但是换到别人电脑上，可能会报错：

```text
ModuleNotFoundError: No module named 'flask'
```

或者：

```text
Python version is incompatible
```

这类问题本质上是 **运行环境不一致**。

---

### 2.2 Docker 解决的问题

Docker 的主要作用是解决环境一致性问题。

它可以做到：

| 作用 | 说明 |
|---|---|
| 环境一致 | 开发、测试、部署环境保持一致 |
| 快速部署 | 一条命令即可启动应用 |
| 隔离运行 | 不同项目之间互不影响 |
| 易于迁移 | 本机、服务器、云平台都可以运行 |
| 便于复现 | 其他人可以快速复现你的运行环境 |

例如，一个 Web 项目如果使用 Docker Compose 管理，部署时可能只需要：

```bash
docker compose up -d
```

而不需要手动安装各种依赖。

---

## 3. Docker 与虚拟机的区别

很多初学者容易把 Docker 和虚拟机混淆。二者都可以提供隔离环境，但实现方式不同。

---

### 3.1 虚拟机

虚拟机是在宿主机上模拟出一台完整的计算机。

它通常包括：

```text
应用程序
运行环境
完整操作系统
虚拟硬件
```

常见虚拟机软件包括：

```text
VMware
VirtualBox
Parallels Desktop
Hyper-V
```

虚拟机的优点是隔离性强，可以运行完整操作系统。  
缺点是资源占用较大，启动较慢。

---

### 3.2 Docker 容器

Docker 容器不是完整虚拟机。

容器通常包括：

```text
应用程序
依赖库
运行环境
必要的系统文件
```

但容器通常不包含完整操作系统内核，而是共享宿主机的操作系统内核。

---

### 3.3 Docker 与虚拟机对比

| 对比项 | 虚拟机 | Docker 容器 |
|---|---|---|
| 启动速度 | 慢，通常几十秒到几分钟 | 快，通常几秒 |
| 资源占用 | 高 | 低 |
| 是否包含完整操作系统 | 是 | 否 |
| 隔离程度 | 更强 | 较强 |
| 部署应用 | 较重 | 轻量方便 |
| 常见用途 | 多系统模拟、安全隔离 | 应用部署、开发环境统一 |

可以这样理解：

> 虚拟机像是在电脑里再装一台完整电脑。  
> Docker 像是为应用提供一个独立、轻量、可复制的运行环境。

---

## 4. Docker 的核心概念

学习 Docker 时，需要重点理解以下几个概念：

```text
镜像 Image
容器 Container
Dockerfile
仓库 Registry
数据卷 Volume
网络 Network
Docker Compose
```

它们之间的关系可以简单理解为：

```text
Dockerfile  ->  构建镜像 Image
镜像 Image  ->  运行容器 Container
容器 Container  ->  运行应用程序
数据卷 Volume  ->  保存持久化数据
网络 Network  ->  支持容器之间通信
Compose  ->  管理多个容器
```

---

## 5. 镜像 Image

### 5.1 镜像是什么

镜像可以理解为：

> 一个应用运行环境的模板。

常见镜像包括：

```bash
nginx:latest
mysql:8.0
python:3.10
ubuntu:22.04
redis:7
```

镜像中通常包含：

```text
基础系统文件
运行时环境
程序依赖
应用代码
默认启动命令
```

例如 `python:3.10` 镜像中已经包含 Python 3.10 环境。

---

### 5.2 镜像和容器的关系

镜像和容器的关系类似于：

| 概念 | 类比 |
|---|---|
| 镜像 | 类 / 模板 / 安装包 |
| 容器 | 对象 / 实例 / 正在运行的程序 |

一个镜像可以启动多个容器。  
例如，一个 `nginx` 镜像可以启动多个独立的 Nginx 容器。

---

### 5.3 拉取镜像

从 Docker Hub 拉取镜像：

```bash
docker pull nginx
```

拉取指定版本：

```bash
docker pull nginx:1.25
docker pull mysql:8.0
docker pull python:3.10
```

查看本地已有镜像：

```bash
docker images
```

---

## 6. 容器 Container

### 6.1 容器是什么

容器是镜像运行起来之后的实例。

例如：

```bash
docker run nginx
```

这条命令会基于 `nginx` 镜像创建并运行一个容器。

---

### 6.2 查看容器

查看正在运行的容器：

```bash
docker ps
```

查看所有容器，包括已经停止的：

```bash
docker ps -a
```

---

### 6.3 停止容器

```bash
docker stop 容器ID或容器名
```

例如：

```bash
docker stop my-nginx
```

---

### 6.4 启动已停止的容器

```bash
docker start 容器ID或容器名
```

例如：

```bash
docker start my-nginx
```

---

### 6.5 删除容器

```bash
docker rm 容器ID或容器名
```

例如：

```bash
docker rm my-nginx
```

需要注意：

> 删除容器不会自动删除镜像。  
> 容器是运行实例，镜像是创建容器的模板。

---

## 7. Docker 的基本使用流程

Docker 的基本使用流程如下：

```text
1. 拉取镜像
2. 创建并运行容器
3. 查看容器状态
4. 查看日志
5. 进入容器调试
6. 停止容器
7. 删除容器
```

例如运行一个 Nginx 容器：

```bash
docker pull nginx
docker run -d --name my-nginx -p 8080:80 nginx
docker ps
docker logs my-nginx
docker stop my-nginx
docker rm my-nginx
```

---

## 8. 常用 Docker 命令

### 8.1 镜像相关命令

查看本地镜像：

```bash
docker images
```

拉取镜像：

```bash
docker pull 镜像名
```

删除镜像：

```bash
docker rmi 镜像名或镜像ID
```

示例：

```bash
docker pull nginx
docker images
docker rmi nginx
```

---

### 8.2 容器相关命令

创建并运行容器：

```bash
docker run 镜像名
```

后台运行容器：

```bash
docker run -d 镜像名
```

给容器命名：

```bash
docker run -d --name my-nginx nginx
```

查看正在运行的容器：

```bash
docker ps
```

查看所有容器：

```bash
docker ps -a
```

停止容器：

```bash
docker stop 容器名
```

启动已停止容器：

```bash
docker start 容器名
```

重启容器：

```bash
docker restart 容器名
```

删除容器：

```bash
docker rm 容器名
```

---

### 8.3 日志相关命令

查看容器日志：

```bash
docker logs 容器名
```

持续查看日志：

```bash
docker logs -f 容器名
```

示例：

```bash
docker logs -f my-nginx
```

---

### 8.4 进入容器

进入正在运行的容器：

```bash
docker exec -it 容器名 bash
```

如果容器中没有 `bash`，可以使用：

```bash
docker exec -it 容器名 sh
```

示例：

```bash
docker exec -it my-nginx bash
```

---

## 9. Dockerfile

### 9.1 Dockerfile 是什么

Dockerfile 是一个文本文件，用来描述如何构建一个 Docker 镜像。

它可以理解为：

> 制作镜像的说明书。

例如，一个 Python 项目的目录结构如下：

```text
my-app/
├── app.py
├── requirements.txt
└── Dockerfile
```

---

### 9.2 一个基础 Dockerfile 示例

```dockerfile
FROM python:3.10

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

### 9.3 Dockerfile 常用指令解释

#### FROM

```dockerfile
FROM python:3.10
```

表示以 `python:3.10` 镜像作为基础镜像。

---

#### WORKDIR

```dockerfile
WORKDIR /app
```

设置容器内部的工作目录。  
后续命令默认都会在 `/app` 目录下执行。

---

#### COPY

```dockerfile
COPY requirements.txt .
```

表示将宿主机当前目录下的 `requirements.txt` 复制到容器当前目录。

```dockerfile
COPY . .
```

表示将当前目录下的所有内容复制到容器当前目录。

---

#### RUN

```dockerfile
RUN pip install -r requirements.txt
```

表示在构建镜像时执行命令，通常用于安装依赖。

---

#### EXPOSE

```dockerfile
EXPOSE 5000
```

声明容器内部服务监听的端口。  
注意：`EXPOSE` 只是声明端口，并不会自动完成宿主机端口映射。

---

#### CMD

```dockerfile
CMD ["python", "app.py"]
```

表示容器启动后默认执行的命令。

---

### 9.4 构建镜像

在 Dockerfile 所在目录执行：

```bash
docker build -t my-python-app .
```

参数说明：

```text
-t my-python-app  给镜像命名
.                 使用当前目录作为构建上下文
```

---

### 9.5 运行自定义镜像

```bash
docker run my-python-app
```

如果应用需要暴露端口：

```bash
docker run -d -p 5000:5000 --name my-app my-python-app
```

---

## 10. 使用 Docker 部署一个 Flask 项目

### 10.1 项目结构

```text
flask-demo/
├── app.py
├── requirements.txt
└── Dockerfile
```

---

### 10.2 app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def index():
    return "Hello Docker!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

这里需要注意：

```python
host="0.0.0.0"
```

不能只监听默认的 `127.0.0.1`。  
因为如果 Flask 只监听容器内部的 `127.0.0.1`，宿主机可能无法访问容器中的服务。

---

### 10.3 requirements.txt

```text
flask
```

---

### 10.4 Dockerfile

```dockerfile
FROM python:3.10

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

---

### 10.5 构建镜像

```bash
docker build -t flask-demo .
```

---

### 10.6 运行容器

```bash
docker run -d --name flask-demo-container -p 5000:5000 flask-demo
```

---

### 10.7 访问服务

在浏览器中访问：

```text
http://localhost:5000
```

如果一切正常，可以看到：

```text
Hello Docker!
```

---

## 11. 数据卷 Volume

### 11.1 为什么需要数据卷

容器本身是可以删除的。

如果数据保存在容器内部，删除容器后数据可能会丢失。

例如 MySQL 容器中的数据库文件，如果没有做持久化处理，删除容器可能导致数据库数据丢失。

因此，Docker 提供了数据卷机制。

---

### 11.2 数据卷的作用

数据卷用于将数据保存到容器外部。

常见用途包括：

```text
保存数据库数据
保存日志文件
挂载配置文件
保存用户上传文件
```

---

### 11.3 使用数据卷运行 MySQL

```bash
docker run -d \
  --name mysql-demo \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -p 3306:3306 \
  -v mysql-data:/var/lib/mysql \
  mysql:8.0
```

其中：

```bash
-v mysql-data:/var/lib/mysql
```

表示：

```text
Docker 数据卷 mysql-data  ->  容器内 /var/lib/mysql
```

MySQL 的数据会保存在 `mysql-data` 数据卷中。  
即使删除容器，只要不删除数据卷，数据仍然可以保留。

---

### 11.4 查看数据卷

查看所有数据卷：

```bash
docker volume ls
```

查看某个数据卷详情：

```bash
docker volume inspect mysql-data
```

删除数据卷：

```bash
docker volume rm mysql-data
```

---

## 12. 目录挂载 Bind Mount

除了 Docker 管理的数据卷，也可以将宿主机目录直接挂载到容器中。

例如：

```bash
docker run -d \
  --name nginx-demo \
  -p 8080:80 \
  -v /Users/yourname/web:/usr/share/nginx/html \
  nginx
```

含义是：

```text
宿主机 /Users/yourname/web
挂载到容器 /usr/share/nginx/html
```

这样修改宿主机目录中的网页文件，容器中的 Nginx 会直接读取更新后的内容。

---

## 13. Docker 网络

### 13.1 为什么需要 Docker 网络

多个容器之间经常需要通信。

例如：

```text
Web 容器需要连接 MySQL 容器
后端容器需要连接 Redis 容器
Nginx 容器需要转发请求到后端容器
```

这时可以使用 Docker 网络。

---

### 13.2 创建网络

```bash
docker network create my-network
```

---

### 13.3 将 MySQL 容器加入网络

```bash
docker run -d \
  --name mysql-demo \
  --network my-network \
  -e MYSQL_ROOT_PASSWORD=123456 \
  mysql:8.0
```

---

### 13.4 将 Web 容器加入同一网络

```bash
docker run -d \
  --name web-demo \
  --network my-network \
  -p 5000:5000 \
  web-demo
```

如果两个容器处于同一个 Docker 网络中，Web 容器可以直接通过容器名访问 MySQL：

```text
mysql-demo
```

而不需要手动查找 IP 地址。

---

## 14. Docker Compose

### 14.1 Docker Compose 是什么

如果一个项目只有一个容器，使用 `docker run` 即可。

但实际项目经常包含多个服务：

```text
Web 后端
MySQL 数据库
Redis 缓存
Nginx 反向代理
消息队列
```

如果每个服务都用一长串 `docker run` 命令启动，会非常麻烦。

Docker Compose 可以通过一个配置文件统一管理多个容器。

---

### 14.2 docker-compose.yml 示例

下面是一个 Flask + MySQL 项目的 Compose 示例：

```yaml
services:
  web:
    build: .
    container_name: flask-web
    ports:
      - "5000:5000"
    depends_on:
      - mysql

  mysql:
    image: mysql:8.0
    container_name: flask-mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_DATABASE: demo
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```

---

### 14.3 启动项目

```bash
docker compose up -d
```

---

### 14.4 查看运行状态

```bash
docker compose ps
```

---

### 14.5 查看日志

```bash
docker compose logs -f
```

查看某个服务日志：

```bash
docker compose logs -f web
```

---

### 14.6 停止项目

```bash
docker compose down
```

---

### 14.7 停止并删除数据卷

```bash
docker compose down -v
```

需要注意：

```bash
docker compose down
```

通常不会删除数据卷。

```bash
docker compose down -v
```

会删除数据卷，数据库数据也可能被删除。

---

## 15. 常见实用示例

### 15.1 快速启动 Nginx

```bash
docker run -d --name nginx-demo -p 8080:80 nginx
```

访问：

```text
http://localhost:8080
```

停止容器：

```bash
docker stop nginx-demo
```

删除容器：

```bash
docker rm nginx-demo
```

---

### 15.2 快速启动 Redis

```bash
docker run -d --name redis-demo -p 6379:6379 redis:7
```

进入 Redis 客户端：

```bash
docker exec -it redis-demo redis-cli
```

测试：

```bash
ping
```

如果返回：

```text
PONG
```

说明 Redis 正常运行。

---

### 15.3 快速启动 MySQL

```bash
docker run -d \
  --name mysql-demo \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -e MYSQL_DATABASE=testdb \
  -p 3306:3306 \
  -v mysql-data:/var/lib/mysql \
  mysql:8.0
```

进入 MySQL：

```bash
docker exec -it mysql-demo mysql -uroot -p
```

输入密码：

```text
123456
```

---

### 15.4 快速启动 Ubuntu 容器

```bash
docker run -it ubuntu:22.04 bash
```

退出容器：

```bash
exit
```

后台运行 Ubuntu 容器：

```bash
docker run -dit --name ubuntu-demo ubuntu:22.04 bash
```

进入后台运行的 Ubuntu 容器：

```bash
docker exec -it ubuntu-demo bash
```

---

## 16. Docker 常见参数解释

### 16.1 `-d`

表示后台运行容器：

```bash
docker run -d nginx
```

---

### 16.2 `--name`

给容器指定名称：

```bash
docker run --name my-nginx nginx
```

---

### 16.3 `-p`

端口映射：

```bash
docker run -p 8080:80 nginx
```

含义是：

```text
宿主机端口:容器端口
```

也就是：

```text
宿主机 8080 端口  ->  容器 80 端口
```

---

### 16.4 `-v`

挂载数据卷或目录：

```bash
docker run -v mysql-data:/var/lib/mysql mysql
```

或者：

```bash
docker run -v /本机目录:/容器目录 nginx
```

---

### 16.5 `-e`

设置环境变量：

```bash
docker run -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0
```

很多官方镜像都会通过环境变量进行初始化配置。

---

### 16.6 `-it`

交互式终端：

```bash
docker run -it ubuntu bash
```

其中：

```text
-i 表示保持输入
-t 表示分配终端
```

通常二者一起使用。

---

## 17. Docker 的典型使用场景

### 17.1 本地开发环境

可以使用 Docker 快速启动开发所需的数据库、中间件和服务，例如：

```bash
docker run -d -p 3306:3306 mysql:8.0
docker run -d -p 6379:6379 redis:7
```

这样可以避免在本机直接安装大量软件，保持系统环境干净。

---

### 17.2 Web 项目部署

对于 Flask、Django、Spring Boot、Node.js、Go 等 Web 项目，可以通过 Dockerfile 将项目打包成镜像，然后在服务器上部署。

典型部署命令：

```bash
docker compose up -d
```

---

### 17.3 课程实验环境

在计算机网络、数据库、操作系统、人工智能等课程实验中，Docker 可以用于提供统一实验环境。

优点包括：

```text
减少环境配置时间
避免不同电脑环境差异
方便教师和学生复现实验结果
```

---

### 17.4 机器学习与深度学习环境

机器学习项目经常依赖特定版本的：

```text
Python
PyTorch
TensorFlow
CUDA
cuDNN
OpenCV
Ultralytics
```

Docker 可以帮助固定这些依赖版本，避免环境冲突。

例如 YOLO 项目中，Docker 常用于统一：

```text
CUDA 版本
PyTorch 版本
OpenCV 版本
模型运行环境
```

---

## 18. Docker 常见问题与排查

### 18.1 容器端口和宿主机端口混淆

例如：

```bash
docker run -p 8080:80 nginx
```

这里应该访问：

```text
http://localhost:8080
```

而不是：

```text
http://localhost:80
```

因为 `8080` 是宿主机端口，`80` 是容器内部端口。

---

### 18.2 Flask 无法从宿主机访问

如果 Flask 写成：

```python
app.run(port=5000)
```

可能导致容器外部无法访问。

推荐写法：

```python
app.run(host="0.0.0.0", port=5000)
```

---

### 18.3 删除容器后数据丢失

如果数据库没有挂载数据卷，删除容器后数据可能丢失。

推荐使用：

```bash
-v mysql-data:/var/lib/mysql
```

保存数据库数据。

---

### 18.4 镜像和容器占用磁盘空间

查看镜像：

```bash
docker images
```

清理无用资源：

```bash
docker system prune
```

清理更多未使用镜像：

```bash
docker system prune -a
```

注意：

> `docker system prune -a` 可能删除未被使用的镜像，使用前需要确认是否还需要这些镜像。

---

### 18.5 Mac 和 Windows 上的 Docker

在 Linux 上，Docker 更接近原生容器。

在 macOS 和 Windows 上，Docker Desktop 通常通过轻量虚拟机运行 Linux 容器。

因此，在 Mac 或 Windows 上使用 Docker 时，文件挂载、网络、性能表现可能与 Linux 服务器略有差异。

---

## 19. Docker 学习路线

### 19.1 第一阶段：基础命令

需要掌握：

```bash
docker pull
docker images
docker run
docker ps
docker stop
docker rm
docker logs
docker exec
```

目标是能够完成：

```text
拉取镜像
启动容器
查看日志
进入容器
停止容器
删除容器
```

---

### 19.2 第二阶段：Dockerfile

需要掌握：

```dockerfile
FROM
WORKDIR
COPY
RUN
EXPOSE
CMD
```

目标是能够把自己的 Python、Java、Node.js 或 Go 项目打包成镜像。

---

### 19.3 第三阶段：数据卷和网络

需要理解：

```text
为什么容器数据会丢失
如何持久化数据库数据
容器之间如何通信
```

常用命令：

```bash
docker volume ls
docker network ls
docker network create
```

---

### 19.4 第四阶段：Docker Compose

需要学会编写：

```yaml
services:
  app:
    build: .
  mysql:
    image: mysql:8.0
```

目标是能够一键启动完整项目。

---

## 20. 最小必会命令清单

如果只想先掌握最常用的命令，下面这些基本够用：

```bash
# 查看镜像
docker images

# 拉取镜像
docker pull nginx

# 运行容器
docker run -d --name my-nginx -p 8080:80 nginx

# 查看运行中的容器
docker ps

# 查看所有容器
docker ps -a

# 查看日志
docker logs -f my-nginx

# 进入容器
docker exec -it my-nginx bash

# 停止容器
docker stop my-nginx

# 启动容器
docker start my-nginx

# 删除容器
docker rm my-nginx

# 删除镜像
docker rmi nginx

# 清理无用资源
docker system prune
```

---

## 21. 完整练习：用 Docker 启动一个 Nginx 网站

下面通过一个完整练习理解 Docker 的基本使用。

---

### 21.1 创建项目目录

```bash
mkdir docker-nginx-demo
cd docker-nginx-demo
```

---

### 21.2 创建网页文件

```bash
echo "Hello Docker Nginx!" > index.html
```

---

### 21.3 启动 Nginx 并挂载当前目录

macOS / Linux：

```bash
docker run -d \
  --name nginx-demo \
  -p 8080:80 \
  -v $(pwd):/usr/share/nginx/html \
  nginx
```

Windows PowerShell：

```powershell
docker run -d `
  --name nginx-demo `
  -p 8080:80 `
  -v ${PWD}:/usr/share/nginx/html `
  nginx
```

---

### 21.4 浏览器访问

```text
http://localhost:8080
```

如果正常，可以看到：

```text
Hello Docker Nginx!
```

---

### 21.5 修改网页内容

```bash
echo "Docker is useful!" > index.html
```

刷新浏览器，页面内容会发生变化。

---

### 21.6 停止并删除容器

```bash
docker stop nginx-demo
docker rm nginx-demo
```

---

## 22. 总结

Docker 的核心价值是：

> 将应用程序及其运行环境打包到统一、可复现、可迁移的容器中。

它不是编程语言，也不是传统意义上的虚拟机，而是一种应用环境管理和部署工具。

对于日常开发和学习来说，Docker 最常用于：

```text
统一开发环境
快速部署项目
运行数据库和中间件
隔离不同项目依赖
复现 AI / 后端 / 实验环境
在服务器上部署服务
```

入门 Docker 需要重点掌握：

```text
镜像 Image
容器 Container
Dockerfile
数据卷 Volume
网络 Network
Docker Compose
```

以及以下基本能力：

```text
会拉取镜像
会运行容器
会映射端口
会挂载目录
会查看日志
会进入容器
会写 Dockerfile
会使用 Docker Compose
```

掌握这些内容后，就可以完成大部分 Docker 日常使用任务。
