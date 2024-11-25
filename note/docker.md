#### docker 

docker run -it -v $(pwd):/app my_project_image bash 挂载项目到容器中







[入门文档](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

[docker-hub](https://hub.docker.com/_/python)

##### 常用命令

- **查看正在运行的容器：**

  docker ps

- **查看所有容器（包括已停止的）**：

  docker ps -a

- **停止容器：**

  docker stop <container_id or container_name>

  使用容器的 ID 或者名称来停止容器。

- **删除容器：**

  docker rm <container_id or container_name>

  使用容器的 ID 或者名称来停止容器。

- 

- **要指定容器的名称，**你可以使用 `--name` 选项，后跟你想要的容器名称。例如：

  ```
  docker run --name my_container <image_name>
  ```

  这样会创建一个名为 `my_container` 的容器。

  要进行端口映射，你可以使用 `-p` 选项，后跟要映射的主机端口和容器端口。例如：

  ```
  docker run -p 8080:80 <image_name>
  ```

  这将主机的 8080 端口映射到容器的 80 端口。

  创建容器并且不依赖app.py文件

  ```
  docker run --name container_1 -p 8081:8082 image_1 /bin/bash
  ```

  创建容器且不立即退出：

  ```
  docker run --name cont_1 -itd -p 8001:8002 ubuntu
  ```

  ```
  sudo docker run -it --name c_4 --gpus all -v $(pwd):/app -itd -p 2224:22 --privileged image4 /bin/bash
  
  ```

  ```
  // 容器中配置
  sudo iptables -A INPUT -p tcp --dport 2225 -j ACCEPT
  ```

  配置wsl端口转发

  ```
  netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=2224 connectaddress=172.30.131.110 connectport=2224
  
  ```

  查看所有端口转发

  ```
   netsh interface portproxy show all
  ```

  

  参数含义分别为

  - `--name ubuntu` 给新的容器命名为 `ubuntu`
  - `-i` 相当于 `--interactive` 使用标准输入与容器交互，保持对容器的标准输入开放
  - `-t` 相当于 `--tty` 表示给容器分配虚拟终端
  - `-d` 相当于 `--detach` 表示容器在后台运行
  - 最后的 `ubuntu` 表示使用的镜像名称

  进入容器：

  ```
  docker exec -it cont_1 /bin/bash
  ```

  关闭和启动容器：

  ```
  docker stop cont_1
  docker start cont_1
  ```

  安装服务：

  ```
  apt update
  apt install vim openssh-server
  新建下载路径 cd ~
  wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py38_4.9.2-Linux-x86_64.sh
  chmod u+x Miniconda3-py38_4.9.2-Linux-x86_64.sh
  ./Miniconda3-py38_4.9.2-Linux-x86_64.sh
  ```

  查看：

  ps -e|grep ssh

  启动ssh:

  sudo /etc/init.d/ssh start

- **docker启动命令,docker重启命令,docker关闭命令**

  启动        systemctl start docker

  守护进程重启   sudo systemctl daemon-reload

  重启docker服务   systemctl restart  docker

  重启docker服务  sudo service docker restart

  关闭docker service docker stop

  关闭docker systemctl stop docker
  

关闭wsl：

```
cmd:
 wsl --shutdown
```

##### dockerfile

基础的dockerfile

```
FROM python:3

WORKDIR /usr/src/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD [ "python", "./app.py" ]
```

然后在dockerfile在同级目录下运行

```
docker build -t my_name .
```

然后使用 

> docker image

命令查看所有的镜像





1. 创建docker 容器

2. 设置wsl2的2227端口

3. 将window的ip地址2227端口和wsl2的ip地址的2227端口映射

   ```
   netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=2227 connectaddress=172.30.131.110 connectport=2227
   ```

   