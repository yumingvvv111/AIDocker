## 部署 Django 使用 Nginx, Celery, Redis, Postgresql, Docker

### 基本用法
1. 运行 `make build` 
2. 再运行 `make up` 

### 预览
项目代码在 `src` 目录中. 开始运行时会开放 `8000` 端口:


### 所有命令
1. `make up` 启动项目和容器
2. `make build` 构建项目
3. `make start` 只启动容器
4. `make stop` 停止容器
5. `make shell-web` web容器shell脚本.
6. `make shell-db` 数据库容器的shell.
7. `make shell-nginx` nginx的shell.
8.  `make collectstatic` 放置静态文件.
9.  `make log-web` web日志.
10. `make log-db` 数据库日志.
11. `make log-nginx`nginx日志.
12. `make restart` 重启容器.
