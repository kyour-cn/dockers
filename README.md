## Docker & Podman常用命令

> 镜像&容器管理

```bash
podman pull $image

podman ps
podman ps -a

podman rmi $image
podman rm $container

#创建并启动镜像
podman run -it -p $主机端口:$容器端口 -v $localPath:$coPath --name="$container" $image

#容器创建镜像
podman commit $container $image
```

> Dockerfile构建镜像

```bash
podman build -t $image .
```

> 容器操作
```bash
podman start $container
podman stop $container

podman exec $container $cmd
podman exec -it $container bash
```

> 复制文件
```bash
podman cp $path $container:$coPath
podman cp $container:$coPath $path
```

> 创建网络

```bash
docker network create --driver bridge --subnet=172.10.0.0/24 --gateway=172.10.0.1 net-sword
#使用 --network=net-sword --ip=172.10.0.x
docker network {ls rm}
```

## 个人常用操作
```text
nginx容器
docker run -itd -p 80:80 -p 443:443 --network=net-sword --ip=172.10.0.10 \
-v /data:/data --name="nginx-sword" kyour/nginx-sword

php-fpm容器
docker run -itd -p 9000:9000 --network=net-sword --ip=172.10.0.11 -v /data:/data --name="php-sword" kyour/php-sword

mysql容器
docker run -itd -p 3306:3306 -e MYSQL_ROOT_PASSWORD=168168 --privileged=true \
--network=net-sword --ip=172.10.0.12 \
-v /data/mysql/data:/var/lib/mysql \
-v /data/mysql/files:/data/mysql/files \
--name="mysql-sword" mysql:8.0

修改mysql的root密码
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'pass';
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'pass';
flush privileges;

redis容器 -需要现准备好redis.conf
docker run  -itd -p 6379:6379 -v /data/redis/data:/data \
--network=net-sword --ip=172.10.0.13 \
-v /data/redis/redis.conf:/etc/redis/redis.conf \
 --name="redis-sword" redis redis-server /etc/redis/redis.conf

#修改redis配置
bind 127.0.0.1 #注释掉这部分
protected-mode no #默认yes，开启保护模式，限制为本地访问
daemonize no#默认no，改为yes意为以守护进程方式启动
dir  ./ #输入本地redis数据库存放文件夹（可选）
appendonly yes #redis持久化（可选）
```
