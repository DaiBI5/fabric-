# 阿里云服务器搭建fabric

> 阿里云服务器 Ubuntu18  下载的fabric版本为2.4

## 1. 安装GO语言

```shell
# 使用weget  获取go1.17.4语言安装包
wget https://dl.google.com/go/go1.17.4.linux-amd64.tar.gz
# 将下载好的安装包解压到 /usr/local
sudo tar zxvf go1.17.4.linux-amd64.tar.gz -C /usr/local
# 创建Go目录
mkdir $HOME/go
# 配置环境变量
vim ~/.bashrc
# 增加.bashrc文件下面增添环境变量，保存并退出
export GOROOT=/usr/local/go
export GOPATH=$HOME/gop
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
# 使环境变量生效  
. ~/.bashrc
# 检测go是否安装好
go version
```

 

## 2. 安装Docker和Docker-compose

- 安装Docker (使用国内 daocloud 一键安装命令)

  ```shell
  curl -sSL https://get.daocloud.io/docker | sh
  ```

  将用户添加到docker用户组

  ```shell
  sudo groupadd docker #添加docker用户组
  
  sudo gpasswd -a $USER docker #将登陆用户加入到docker用户组中
  
  newgrp docker #更新用户组
  
  systemctl enable docker #开机自动启动docker
  
  docker version #验证是否安装成功
  ```

- 安装Docker-compose(使用使用国内 daocloud 一键安装)

  ```shell
  sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  
  # 为其分配权限
  sudo chmod +x /usr/local/bin/docker-compose
  
  #检查是否安装成功
  docker-compose -v
  ```

  

## 3. 下载fabric-samples、fabric镜像、fabric-ca镜像、二进制文件

> 直接使用官方的脚本下载十分缓慢，所以需要更新docker镜像源   都试了一下，就163源比较好用

-  更换docker镜像源

    ```shell
    vim /etc/docker/daemon.json #进入编辑这个配置文件
    # 修改配置文件为如下
    {
      "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
    }

    systemctl daemon-reload #使配置文件生效

    service docker restart #重启docker服务

    docker search nginx #测试是否成功
    ```

- 使用官方脚本下载

  ```shell
  # 我使用的是如下脚本 下载最新版
  # 将如下脚本复制到服务器上，写进一个.sh文件（记得修改权限才能运行）
  https://raw.githubusercontent.com/hyperledger/fabric/master/scripts/bootstrap.sh
  
  # 或者可以使用官方提供的   执行以下命令 自动安装
  curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.0 1.4.7
  ```
  

> <font color="red">之后学习如何自己下载安装镜像和二进制文件</font>
>
> 可以参考[这篇博文](https://blog.csdn.net/oheyec_/article/details/104068917)



**至此环境已经成功搭建**
