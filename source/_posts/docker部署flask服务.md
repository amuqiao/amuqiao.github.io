---
title: Docker部署flask服务
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-25
img:
password:
summary:
tags:
- docker
- flask
categories:
- 运维

---
> docker部署flask服务示例

<!--more-->

首先推荐阅读Flask 官方文档[欢迎来到 Flask 的世界](https://dormousehole.readthedocs.io/en/latest/)快速了解Flask架构

### 快速搭建一个flask服务
一个最小的应用
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
	return "Hello World!"

if __name__ == '__main__':
	app.run(host='0.0.0.0')

```
运行demo
```
$ export FLASK_APP=app.py
$ flask run
 * Running on http://127.0.0.1:5000/
```
或者`python -m flask`
```
$ export FLASK_APP=app.py
$ python -m flask run
 * Running on http://127.0.0.1:5000/
```
或者
```
✗ python3 app.py
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```
外部可见的服务器
```
flask run --host=0.0.0.0  # 可以让服务器被公开访问
```
调试模式
```
# 如果需要打开所有开发功能（包括调试模式），那么要在运行服务器之前导出 FLASK_ENV 环境变量并把其设置为 development
$ export FLASK_ENV=development
$ flask run
```

### 项目文件
[代码示例下载](https://github.com/docker/awesome-compose/tree/master/flask)
```
.
├── README.md
├── app
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
└── docker-compose.yaml
```

### docker 部署服务
```
 ✗ cd flask-demo/app
 ✗ docker build -t="flask-demo:v1" .
[+] Building 26.3s (11/11) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                0.0s
 => => transferring dockerfile: 296B                                                                                                                                0.0s
 => [internal] load .dockerignore                                                                                                                                   0.0s
 => => transferring context: 2B                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/python:3.7-alpine                                                                                               26.1s
 => [auth] library/python:pull token for registry-1.docker.io                                                                                                       0.0s
 => [internal] load build context                                                                                                                                   0.0s
 => => transferring context: 697B                                                                                                                                   0.0s
 => [1/5] FROM docker.io/library/python:3.7-alpine@sha256:39ad96c8188f69d513ded616433396a9ab90e061ca85d8eacf6514fa27ec3d40                                          0.0s
 => => resolve docker.io/library/python:3.7-alpine@sha256:39ad96c8188f69d513ded616433396a9ab90e061ca85d8eacf6514fa27ec3d40                                          0.0s
 => CACHED [2/5] WORKDIR /app                                                                                                                                       0.0s
 => CACHED [3/5] COPY requirements.txt /app                                                                                                                         0.0s
 => CACHED [4/5] RUN pip3 install -r requirements.txt --no-cache-dir                                                                                                0.0s
 => [5/5] COPY . /app                                                                                                                                               0.1s
 => exporting to image                                                                                                                                              0.0s
 => => exporting layers                                                                                                                                             0.0s
 => => writing image sha256:9fc116d3008819bf171b4cb2a10957e842b77aa24bde2582d737040cd6f9eaec                                                                        0.0s
 => => naming to docker.io/library/flask-demo:v1   
✗ docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
flask-demo           v1        9fc116d30088   3 minutes ago   51.6MB
```
启动容器

- `-p `指定端口映射，格式为：主机(宿主)端口:容器端口
- `-d`: 后台运行容器，并返回容器ID；
- `-i`: 以交互模式运行容器，通常与 -t 同时使用；
- `-P`: 随机端口映射，容器内部端口随机映射到主机的端口
- `-p`: 指定端口映射，格式为：主机(宿主)端口:容器端口
- `-t`: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
```
 ✗ docker run -d -p 5000:5000 flask-demo:v1
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```
![hello world](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/r1MVwF.png)

打开浏览器输入[localhost:5000](localhost:5000)

关闭容器
```
✗ docker ps
CONTAINER ID   IMAGE           COMMAND            CREATED              STATUS          PORTS                    NAMES
8c87f5f494b8   flask-demo:v1   "python3 app.py"   About a minute ago   Up 10 seconds   0.0.0.0:5000->5000/tcp   nervous_bassi
(venv) ➜  app git:(main) ✗ docker stop 8c87f5f494b8
8c87f5f494b8
(venv) ➜  app git:(main) ✗ docker start 8c87f5f494b8
8c87f5f494b8
```


### [docker-compose](http://www.dockerinfo.net/docker-compose-%e9%a1%b9%e7%9b%ae) 部署服务
> Docker Compose 是 Docker 官方编排（Orchestration）项目之一，负责快速在集群中部署分布式应用。

```
✗ docker-compose up
Building web
[+] Building 16.4s (11/11) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                0.0s
 => => transferring dockerfile: 120B                                                                                                                                0.0s
 => [internal] load .dockerignore                                                                                                                                   0.0s
 => => transferring context: 2B                                                                                                                                     0.0s
 => [internal] load metadata for docker.io/library/python:3.7-alpine                                                                                               16.3s
 => [auth] library/python:pull token for registry-1.docker.io                                                                                                       0.0s
 => [internal] load build context                                                                                                                                   0.0s
 => => transferring context: 505B                                                                                                                                   0.0s
 => [1/5] FROM docker.io/library/python:3.7-alpine@sha256:39ad96c8188f69d513ded616433396a9ab90e061ca85d8eacf6514fa27ec3d40                                          0.0s
 => => resolve docker.io/library/python:3.7-alpine@sha256:39ad96c8188f69d513ded616433396a9ab90e061ca85d8eacf6514fa27ec3d40                                          0.0s
 => CACHED [2/5] WORKDIR /app                                                                                                                                       0.0s
 => CACHED [3/5] COPY requirements.txt /app                                                                                                                         0.0s
 => CACHED [4/5] RUN pip3 install -r requirements.txt --no-cache-dir                                                                                                0.0s
 => CACHED [5/5] COPY . /app                                                                                                                                        0.0s
 => exporting to image                                                                                                                                              0.0s
 => => exporting layers                                                                                                                                             0.0s
 => => writing image sha256:9fc116d3008819bf171b4cb2a10957e842b77aa24bde2582d737040cd6f9eaec                                                                        0.0s
 => => naming to docker.io/library/flask-demo_web                                                                                                                   0.0s
Successfully built 9fc116d3008819bf171b4cb2a10957e842b77aa24bde2582d737040cd6f9eaec
WARNING: Image for service web was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating flask-demo_web_1 ... done
Attaching to flask-demo_web_1
web_1  |  * Serving Flask app "app" (lazy loading)
web_1  |  * Environment: production
web_1  |    WARNING: This is a development server. Do not use it in a production deployment.
web_1  |    Use a production WSGI server instead.
web_1  |  * Debug mode: off
web_1  |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)

^CGracefully stopping... (press Ctrl+C again to force)
Stopping flask-demo_web_1 ... done
(venv) ➜  flask-demo git:(main) ✗ docker ps -a
CONTAINER ID   IMAGE                COMMAND                  CREATED          STATUS                        PORTS                    NAMES
2e48108b8e1d   flask-demo_web       "python3 app.py"         6 minutes ago    Exited (137) 9 seconds ago                             flask-demo_web_1
```
`ctrl+c`终止服务,使用`docker ps -a`查看所有容器，再次启动容器使用`docker start 容器id`
```
✗ docker start 2e48108b8e1d
2e48108b8e1d
(venv) ➜  flask-demo git:(main) ✗ docker ps
CONTAINER ID   IMAGE            COMMAND            CREATED         STATUS          PORTS                    NAMES
2e48108b8e1d   flask-demo_web   "python3 app.py"   9 minutes ago   Up 16 seconds   0.0.0.0:5000->5000/tcp   flask-demo_web_1
```

up参数

- 构建，（重新）创建，启动，链接一个服务相关的容器。
- 链接的服务都将会启动，除非他们已经运行。
- 默认情况， `docker-compose up` 将会整合所有容器的输出，并且退出时，所有容器将会停止。
- 如果使用 `docker-compose up -d` ，将会在后台启动并运行所有的容器。
- 默认情况，如果该服务的容器已经存在， `docker-compose up` 将会停止并尝试重新创建他们（保持使用`volumes-from` 挂载的卷），以保证 `docker-compose.yml` 的修改生效。如果你不想容器被停止并重新创建，可以使用 `docker-compose up --no-recreate`。如果需要的话，这样将会启动已经停止的容器。

### 参考文档
- [一些docker-compose 部署示例](https://github.com/docker/awesome-compose)
- [docker-compose部署flask](https://github.com/docker/awesome-compose/tree/master/flask) 