---
layout:     post
title:      Docker
subtitle:   Docker 安装单机版 Portainer Docker 管理面板
date:       2019-11-26
author:     hfming
header-img: img/首页标题背景.jpg
catalog: true
tags:
    - 
---

# 1.环境准备
## 1.1.实验条件
| 主机内网 IP | 主机名 |
|  ----    |  ----  |
| 172.16.134.130 | hfm2G |  
| 172.16.69.107 | hfm1G |
| 172.17.0.13 | VM_0_13_centos |
如果不知到主机名可以输入以下代码
```Linux
# hostnamectl  // 查询主机信息
# hostnamectl set-hostname newName  // 设置新主机名
```
## 1.2.修改 hosts
三台机子都打开 /etc/hosts 文件，修改hosts
```linux
vi /etc/hosts
```
添加如下代码
```Linux
172.16.69.107 hfm1G hfm1G
172.16.134.130 hfm2G hfm2G
172.17.0.13 VM_0_13_centos VM_0_13_centos
```
![修改hosts](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/%E4%BF%AE%E6%94%B9hosts.PNG)
# 2.修改 docker 监听端口
打开工作机(hfm1,hfm2) /lib/systemd/system/docker.service 文件，修改docker监听端口
```Linux
# vim /lib/systemd/system/docker.service
```
```Linxu
# ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```
![修改docker监听端口](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/%E4%BF%AE%E6%94%B9docker%E7%9B%91%E5%90%AC%E7%AB%AF%E5%8F%A3.PNG )
之后重启工作机(hfm1,hfm2)docker
```linux
# systemctl daemon-reload   // 使配置文件生效
# systemctl restart docker
```
# 3.初始化 Swarm
```Linux
# docker swarm init --advertise-addr 172.17.0.13  // 选择 122.51.54.198 作为管理主机
```
![初始化Swarm](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/%E5%88%9D%E5%A7%8B%E5%8C%96Sware.PNG)
得到如下代码，在其他两台工作机中输入即可
```Linux
# docker swarm join --token SWMTKN-1-0jrvec9a0eo2yz3izizbpdrikx35gryvzilkmf7wu83xxxkw6q-0lczkvm44vlzs7wwclpvieprt 122.51.54.198:2377
```
在manager上执行如下指令，让hfm1与hfm2释放掉manager资格，成为专职的worker。
```Linux
# docker node update hfm1 --role worker
# docker node update hfm2 --role worker
```
最后在管理机查看节点
```Linxu
# docker node ls
```
![docker node ls](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/docker%20node%20ls.PNG)
# 4.Portainer 可视化 Docker Swram 管理
进入到管理机 portainer home 界面
![portainer home 界面](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/portainer%20home.PNG)
选择 local docker 容器，点击进入，点击 endponits, 点击蓝色按键添加 endponit
![add endpoint](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/portainer%20add%20endpoint.PNG)
选择添加类型为 Docker ,输入名称与 IP 地址、端口，最后点击添加即可
![add endpoint](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/portainer%20%E6%B7%BB%E5%8A%A0endponit.PNG)
***
在Home处也可以见到Endpoints
# 5.管理 Endpoint 资源

点击你想管理的Docker服务器
![节点管理界面](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/swarm%20%E8%8A%82%E7%82%B9%E7%AE%A1%E7%90%86.PNG)
节点资源管理
![节点服务](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/swarm%20%E8%8A%82%E7%82%B9%E6%9C%8D%E5%8A%A1.PNG)