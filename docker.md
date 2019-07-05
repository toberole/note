# docker命令
- 查看本地的镜像列表

    docker images

- 搜索“xxxx” 镜像

    docker search xxxx    

- 下载“xxxx”镜像

    docker pull xxxx

- 创建并启动进入容器
<pre>
docker run -it --name "给容器的名字" "下载下来的镜像的名字" ["初始命令",进入容器之后 在容器中执行的命令]

-i：表示以“交互模式”运行容器

-t：表示容器启动后会进入其命令行

-v：表示需要将本地哪个目录挂载到容器中，格式：-v <宿主机目录>:<容器目录>

-d：表示以“守护模式”

-p：表示宿主机与容器的端口映射,-p 8080:80[宿主机的8080与容器的80端口做一个映射 ]

eg:
    docker run -it --name xxx yyy -p 8080:80 ubutun
</pre>

- 查看正在运行的容器

    docker ps

- 查看所有的容器 运行的和没有运行的

    docker ps -a 

- 停止容器
    
    docker stop "容器ID"

- 移除容器

    docker rm [-f] "容器ID"

- 移除镜像

    docker rmi "镜像ID"

- 在运行的容器中执行命令
<pre>
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

OPTIONS说明：

-d :分离模式: 在后台运行

-i :即使没有附加也保持STDIN 打开

-t :分配一个伪终端

eg: 在容器mynginx中以交互模式执行容器内/root/runoob.sh脚本

docker exec -it mynginx /bin/sh /root/runoob.sh

</pre>

- 启动容器

    docker start "容器"

- 停止容器

    docker stop “容器”

- 退出不停止容器
    ctrl+p+q

- 查看容器的原信息

    docker inspect "容器"

- 查看正在运行的容器的IP

    docker inspect --format='{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' "容器"

- 将容器保存为一个新的镜像
docker commit "23c18d958279希望保存为新镜像的容器id" xxx:v0.2


# win7下面docker安装的默认的linux
https://github.com/boot2docker/boot2docker

user:docker

pwd:tcuser


<pre>
docker是Linux虚拟容器技术，用它必须是在Linux上，windows之所以能用 是使用了虚拟机。
在windows7上使用docker：
虚拟的层数：win7->Linux->容器

在做文件共享的时候需要注意：
win7 共享------> linux虚拟机 ------> 容器
</pre>

- SSH登陆
ssh dev@10.134.14.136

- SCP 传递文件到服务器
scp form_localpath user@servername:to_server_pathls
