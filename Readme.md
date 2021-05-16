# 企鹅物流数据统计 Backend (Docker)

## 如何部署

### 1.Clone 本项目

```shell
git clone https://github.com/penguin-statistics/backend-docker.git
cd backend-docker
```

### 1.安装 docker-ce

### 2.安装 docker-compose

```shell
curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

### 3. 下载源代码
修改docker-compose.yml 指定pull branch
`获取源代码方式将会被改变`

### 4.构建并启动容器

```shell
docker-compose build
docker-compose up -d
```

### 5.导入数据库文件

请联系企鹅物流数据统计获取数据库 dumped file

将 **penguin_stats** 文件夹放入 ./mongodb/backup

并使用以下命令导入 (更改密码用户部分)

```shell
docker-compose exec mongo mongorestore -h localhost:27017 -d penguin_stats -u root -p root --authenticationDatabase admin /backup/penguin_stats
```

### 6.创建新用户

用户名密码需要与 penguin-backend/application.yml 对应

```shell
docker-compose exec mongo mongo localhost:27017/penguin_stats -u root -p root --authenticationDatabase admin --eval "db.createUser({user: 'root', pwd: 'root', roles:['dbOwner']})"
```

### 7.重启

```shell
docker-compose restart
```

### 8. Development and Production
`docker-compose.yml` is suitable for production use. `docker-compose.override.yml` will apply additional changes to `docker-compose.yml`.

#### Development
```shell
docker-compose up -d  
```
`docker-compose.override.yml` contains ports exposed on host and mongo backup folder for importing database.
Ports exposed on host are 
* 80 for web service (frontend + backend)
* 8080 for web service (backend only)
* 27017 for mongodb
* 6379 for redis

#### Production

```shell
docker-compose -f docker-compose.yml up -d  
```
This does not include changes in `docker-compose.override.yml`.
