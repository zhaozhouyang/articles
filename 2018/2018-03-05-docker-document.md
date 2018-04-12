# Docker
## 1. 安装
[Docker Install](https://docs.docker.com/install/)  
Docker CE (Community Edition)  
Docker EE (Enterprise Edition)  
我们用CE， 其中CE有两个更新渠道：
1. Stable：每个季度更新
2. Edge：每月更新  

可以从上述链接看到有Desktop安装和云服务器安装各种版本，选择版本安装即可

## 2. 简介
- image: 镜像，可以被执行的打包，包含所有需要运行的内容：运行库、环境变量、配置文件
- container: 容器，image在内存的运行实例，比如通过docker ps可以查看running containers

![Image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/system/container-vs-vm.png)

```
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Excecute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -a -q
```

## 3. 容器
### 3.1 建立Dockerfile
新建一个Dockerfile:
```
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```
### 3.2 建立image
通过
```
docker build -t friendlyhello .
```
建立image  
### 3.3 运行image
通过
```
docker run -p 4000:80 friendlyhello
```
运行image  
### 3.4 给image打标签
```
// docker tag image username/repository:tag
docker tag friendlyhello john/get-started:part2
```
### 3.5 发布image
```
// 'docker login' first
docker push username/repository:tag
```
### 总结
```
docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```
## 4. 服务 Service
一个app的不同部分叫做services；比如一个视频分享网站，有存储数据的service，有解码的service，有上传的service等等
### 4.1 建立docker-compose.yml
```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
    networks:
      - webnet
networks:
  webnet:
```
- services下的web: 表示服务名称
- image: 表示要用的镜像
- replicas: 表示运行5个实例
- limits: 表示每个replicas可用资源
- restart_policy: 每次失败后自动重启
- ports: 类似-p端口映射 左->右 本机->container内部
- web下的networks: 告诉服务web的运行起来的containers(5个),通过叫做"webnet"的网络来进行负载均衡
- networks: 定义webnet，空表示使用默认设置

### 4.2 运行支持负载均衡的app
两行
把docker-compose.yml定义的app的下面的services运行起来
```
docker swarm init
docker stack deploy -c docker-compose.yml getstartedlab
// app的名字叫getstartedlab
```
此时通过访问localhost，可以看到访问不同的container，说明负载均衡在起作用  
此时修改replicas或者limits资源限制后，直接再次运行
```
docker stack deploy -c docker-compose.yml getstartedlab
```
就能立马生效  

一个service下运行的每个container叫做task，通过
```
docker service ps getstartedlab_web
```
可以查看某个service下面的task

> docker stack deploy运行的app叫做getstartedlab，里面有若干的service，其中一个叫做getstartedlab_web，是通过_拼接的

### 4.3 关闭
关闭APP：
```
docker stack rm getstartedlab
```
关闭swarm：
```
docker swarm leave --force
```

### 总结
```
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager
```

## 5. Swarms
- 一个Swarm是一组运行docker的机器集群。
- 每个Swarm通过swarm manager来运行之前熟悉的docker命令。   
- 每一个机器叫做node。  
- Swarm manager有若干的策略来运行容器，比如每次优先运行容器数目最少的机器上；比如保证每个容器在每个机器上都有一个实例等等。
- 只有Swarm manager能执行你的docker命令，或者授权其他机器作为worker加入这个swarm；workder只提供运算能力。

### 5.1 创建机器
安装[Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)后，新建两个虚拟机，名字分别为myvm1和myvm2
```
docker-machine create --driver virtualbox myvm1
docker-machine create --driver virtualbox myvm2
```
然后通过
```
docker-machine ls

// 返回结果如下：
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v17.12.1-ce   
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.12.1-ce  
```
可以看到当前的docker机器和他们的信息  

### 5.2 设置集群

第一个执行"docker swarm init"的机器就行swarm manager  
知道第一个ip是192.168.99.100，通过
```
docker-machine ssh myvm1 "docker swarm init --advertise-addr 192.168.99.100"
```
返回
```
Swarm initialized: current node (cd4qv5t4i96lrrlhz1p1ppwa5) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-2hudzu61erjp6g65jmtfxno3x12iij38is3iudn05hvjfum1m9-7asgpcmu7x243ckub14b3iqio 192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```
可以看到第一个myvm1已经是swarm manager了，并告知，在其他机器上执行
```
docker swarm join --token SWMTKN-1-2hudzu61erjp6g65jmtfxno3x12iij38is3iudn05hvjfum1m9-7asgpcmu7x243ckub14b3iqio 192.168.99.100:2377
即：
docker-machine ssh myvm2 "docker swarm join --token SWMTKN-1-2hudzu61erjp6g65jmtfxno3x12iij38is3iudn05hvjfum1m9-7asgpcmu7x243ckub14b3iqio 192.168.99.100:2377"
```
可以把那台myvm2机器作为worker加入到这个swarm  
然后通过
```
docker-machine ssh myvm1 "docker node ls"
// 返回
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
cd4qv5t4i96lrrlhz1p1ppwa5 *   myvm1               Ready               Active              Leader
sfrnbkso2mxfuere9sl96bk4x     myvm2               Ready               Active         
```
可以在swarm manager（myvm1）上查看节点情况

### 5.3 部署app到swarm集群
#### 5.3.1 连接到虚拟机myvm1的shell
首先，打开shell连接到swarm manager，先运行：
```
docker-machine env myvm1
// 返回
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/zhaozhouyang/.docker/machine/machines/myvm1"
export DOCKER_MACHINE_NAME="myvm1"
# Run this command to configure your shell:
# eval $(docker-machine env myvm1)
```
来查看目标机器myvm1的环境变量以及最后一行的连接命令：
```
eval $(docker-machine env myvm1)
```
等于把那些Export覆盖了本机，后面执行的docker命令就是在myvm1上了（docker_host已经变成了192那个了）（可以看到docker image ls 已经是空的了），可以通过这个验证：
```
docker-machine ls
// 返回
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   *        virtualbox   Running   tcp://192.168.99.100:2376           v17.06.2-ce   
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.06.2-ce   
```
可以看到myvm1的ACTIVE标记了*
#### 5.3.2 部署
和部署在本机一样，需要命令：
```
docker stack deploy -c docker-compose.yml getstartedlab
```
但是这条命令需要在myvm1的这个swarm manager上运行才行，由于5.3.1的配置，目前已经是了，直接运行即可  
通过之前的"docker service ps getstartedlab_web"查看这个service下运行的container(task)  
可以看到被运行在了不同的node上  
通过之前的ip，访问192.168.99.100或者192.168.99.101  
会发现每一个都能随机访问到replicas的5个container，访问谁都一样，逻辑如图：
![image](https://raw.githubusercontent.com/zhaozhouyang/markdown-photots/master/system/swarm-ingress-network.png)
### 5.4 清除和重启
关闭Stack（app）
```
docker stack rm getstartedlab
```
关闭swarm集群
```
docker-machine ssh myvm2 "docker swarm leave" // on the worker
docker-machine ssh myvm1 "docker swarm leave --force" // on the manager
```
修改回环境变量，把docker命令指回本机
```
eval $(docker-machine env -u)
```

### 5.5 其他命令
比如删除机器，关闭机器等
```
docker-machine create --driver virtualbox myvm1 # Create a VM (Mac, Win7, Linux)
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1 # Win10
docker-machine env myvm1                # View basic information about your node
docker-machine ssh myvm1 "docker node ls"         # List the nodes in your swarm
docker-machine ssh myvm1 "docker node inspect <node ID>"        # Inspect a node
docker-machine ssh myvm1 "docker swarm join-token -q worker"   # View join token
docker-machine ssh myvm1   # Open an SSH session with the VM; type "exit" to end
docker node ls                # View nodes in swarm (while logged on to manager)
docker-machine ssh myvm2 "docker swarm leave"  # Make the worker leave the swarm
docker-machine ssh myvm1 "docker swarm leave -f" # Make master leave, kill swarm
docker-machine ls # list VMs, asterisk shows which VM this shell is talking to
docker-machine start myvm1            # Start a VM that is currently not running
docker-machine env myvm1      # show environment variables and command for myvm1
eval $(docker-machine env myvm1)         # Mac command to connect shell to myvm1
& "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env myvm1 | Invoke-Expression   # Windows command to connect shell to myvm1
docker stack deploy -c <file> <app>  # Deploy an app; command shell must be set to talk to manager (myvm1), uses local Compose file
docker-machine scp docker-compose.yml myvm1:~ # Copy file to node's home dir (only required if you use ssh to connect to manager and deploy the app)
docker-machine ssh myvm1 "docker stack deploy -c <file> <app>"   # Deploy an app using ssh (you must have first copied the Compose file to myvm1)
eval $(docker-machine env -u)     # Disconnect shell from VMs, use native docker
docker-machine stop $(docker-machine ls -q)               # Stop all running VMs
docker-machine rm $(docker-machine ls -q) # Delete all VMs and their disk images
```
