# Docker的学习

标签（空格分隔）： 编程工具

---
## Docker在Windows 10上的安装

注意事项：
1. BIOS开启Hyper-V虚拟化功能
2. 控制面板启用Windows Hyper-V功能
3. 自动启动hypervisor
```
bcdedit /set hypervisorlaunchtype auto
```

## Docker在Ubuntu上的安装

>二话不说，先装上Docker，再看他是何方神仙

这里我们还是需要了解Docker的一些版本信息，以便在不同的使用场景中安装不同版本：
1. Community Edition(CE)　个人用户使用
2. Enterprise Edition(EE)　公司或者团队使用,[更多其他功能](https://docs.docker.com/install/overview/);

Docker也有很多种安装方法这里我们使用推荐的Repo安装法，这里有些细节要注意,Docker目前支持在Ubuntu下面版本安装:
1. 17.10(Docker CE 17.11 Edge and higher only)
2. 16.04
3. 14.04

并且对于存储引擎在不同的Ubuntu版本下面也不同，Ubuntu 16.04 及以上默认采用**overlay2**存储引擎，此外还有**aufs**存储引擎可以选择

### 卸载之前的Docker
```
sudo apt-get remove docker docker-egine docker.io
```
虽然卸载了Docker 但是之前在**/var/lib/docker**的文件、配置还在。

#### 配置Repository

1. 更新Ubuntu索引
```
sudo apt-get update
```
2. Install packages to allow apt to use a repository over HTTPS:
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
3. Add Docker’s official GPG key:

>GPG key: 是一个安全密匙用于解密被GNU Privacy Guard (GnuPG)加密过的文件

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Verify that you now have the key with the fingerprint
```
sudo apt-key fingerprint 0EBFCD88
```
4. Setup Repository

Repository 有三种版本Stable、Edge、Test，根据需要选择
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

>  $(lsb_release -cs)用于返回Ubuntu发行版本名称, 且lsb_release -a 能够显示出系统发型版本

#### Install Docker CE

1. 更新索引
```
sudo apt-get update
```

2. 安装Docker-ce
```
sudo apt-get install docker-ce
```
3. 安装指定版本的　Docker

a. 列出Repo中的Docker 版本
```
apt-cache madison docker-ce
```
b. 指定版本号安装
```
sudo apt-get install docker-ce=18.03.0.ce
```

4. 测试Docker是否安装成功

```
sudo docker run hello-world
```

## Docker基本概念

Docker 是用来帮助开发人员或者系统管理员，开发、部署、运行应用的容器。它和VM的区别在于，一个只虚拟了应用运行的环境，另一个虚拟了整个系统，故Docker更轻量化，资源利用率更高.

Docker里面有两个关键概念:
1. image 镜像，能够运行的程序，运行依赖，运行环境和配置文件，包含程序运行的所有需要
2. container 容器，image的运行实例


## Docker 控制
创建image
```
docker build -t ocharbird/virtalbox .
```

镜像操作
```
// 查看镜像列表
docker images
// 删除镜像,先要删除依赖镜像的所有容器【-f 可以强制删除】
docker rmi [name:label]
docker image rm ...
// 清理临时镜像
docker image prune -f   

```

```bash
// 创建容器
docker create [name:label]
// 启动容器
docker start [name:label]
// 创建容器并启动
docker run xxxx
-i // 让容器的标准输入打开
-t // 容器分配一个伪终端
--name // 指定容器的名称
// 电脑的4000端口 映射到容器的80端口
docker run -p 4000:80
// 其它参数默认值和含义
--rm=false: Automatically remove the container when it exits
--privileged=false: Give extended privileges to this container
-e 设置环境变量
-v, --volume=[host-src:]container-dest[:<options>]: Bind mount a volume.
```
守护态运行
```
添加参数 -d 可以让容器在守护态运行
// 可以通过docker ps 来显示守护态运行中的容器
-a 所有的容器
```
容器暂停和重启
```
// 暂停 unpause 恢复
docker pause [name:label]
// 终止 start 重启
docker stop
// 终止后再启动
docker restart
// 删除容器
docker rm [name:label]
```