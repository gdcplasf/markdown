### Apache APISIX 是什么？
***
#### Apache APISIX 是一个动态、实时、高性能的 API 网关，基于 Nginx 网络库和 etcd 实现， 提供负载均衡、动态上游、灰度发布、服务熔断、身份认证、可观测性等丰富的流量管理功能。
&nbsp;
#### 你可以使用 Apache APISIX 来处理传统的南北向流量，以及服务间的东西向流量， 也可以当做 k8s ingress controller 来使用。

<!-- more -->
&nbsp;

### Apache APISIX 的技术架构如下图所示：

&nbsp;

![apisix-structure](https://github.com/zhengwei19003/markdown/blob/main/images/apisix/apisix-structure.png)


&nbsp;

### 1. 安装Openresty、etcd、luarocks

&nbsp;

#### 1.1 Openresty安装
***
```shell
# 添加 OpenResty 源
yum install yum-utils
yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo

# 安装 OpenResty 和 编译工具
yum install -y openresty curl gcc
```

&nbsp;

#### 1.2 etcd安装
***
```shell
wget https://github.com/etcd-io/etcd/releases/download/v3.4.13/etcd-v3.4.13-linux-amd64.tar.gz
tar -xvf etcd-v3.4.13-linux-amd64.tar.gz && \
    cd etcd-v3.4.13-linux-amd64 && \
    cp -a etcd etcdctl /usr/local/bin/
```
#### 启动：`nohup etcd --name=default --data-dir=/var/lib/etcd/default.etcd &`

&nbsp;

#### 1.3 luarocks安装
***
```shell
# 安装luarocks和依赖
yum install -y git luarocks lua-devel
```

&nbsp;

### 1.4. apisix安装

&nbsp;

#### 1.4.1 yum安装
***
```shell
yum install -y https://github.com/apache/apisix/releases/download/2.1/apisix-2.1-0.el7.noarch.rpm
```

#### 1.4.2 启动服务
***
```shell
apisix start
```

&nbsp;

### 2.apisix-dashboard安装
#### apisix-dashboard需要依赖go 1.13+，node 10.23.0+的版本，所以，需要提前安装好go和node。
#### 附下载地址：
***
```
go：

https://studygolang.com/dl/golang/go1.14.13.linux-amd64.tar.gz

node：

https://npm.taobao.org/mirrors/node/v12.19.0/node-v12.19.0-linux-x64.tar.gz
```

&nbsp;

#### 2.1 安装go和node，并配置环境变量。
***
```shell
# go
export GOROOT=/usr/local/golang
export GOPATH=$GOROOT/workspace
export GOBIN=$GOPATH/bin
export PATH=$GOROOT/bin:$GOPATH/bin:$PATH
```

&nbsp;

```
# node
export NODEJS_HOME=/usr/local/nodejs
export PATH=$NODEJS_HOME/bin:$PATH
```

&nbsp;

#### 2.2 使用node安装yarn。
***
```shell

npm install -g yarn
```

#### 2.3 拉取apisix-dashboard源码，使用`make build`构建。

***
```
# Clone the project

git clone https://github.com/apache/apisix-dashboard.git
```

&nbsp;

#### 2.4构建完毕，进入到`output/conf`下，修改`conf.yaml`配置文件，修改连接host地址，执行`nohup ./manager-api &`启动服务。
```shell
conf:
  listen:
    host: 172.16.0.100     # `manager api` listening ip or host name
    port: 9000          # `manager api` listening port
  etcd:
    endpoints:          # supports defining multiple etcd host addresses for an etcd cluster
      - 127.0.0.1:2379
```

&nbsp;

#### 2.5使用`http://172.16.0.100:9000`访问服务。
&nbsp;
![apisix-dashboard](https://github.com/zhengwei19003/markdown/blob/main/images/apisix/apisix-dashboard.png)
