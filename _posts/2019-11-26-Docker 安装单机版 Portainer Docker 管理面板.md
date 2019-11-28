---
layout:     post
title:      Portainer
subtitle:   Docker 安装单机版 Portainer Docker 管理面板
date:       2019-11-26
author:     hfming
header-img: img/首页标题背景.jpg
catalog: true
tags:
    - portainer 
    - docker 
---

# 1.选择镜像
```Linxu
# docker search portainer
```
# 2.下载镜像
```Linux
# docker pull portainer/portainer
```
# 3.运行镜像
```Linux
# docker run -d \
            -p 8000:9000 \
            -v /var/run/docker.sock:/var/run/docker.sock \            
            -v /docker/portainer:/data \            
            --name portainer \            
            --restart=always \            
            portainer/portainer
```
详细参数参考：https://hub.docker.com/r/portainer/portainer
            或https://portainer.readthedocs.io/en/latest/deployment.html
```
-d  # 容器在后台运行
-p 8000:9000    # 宿主机8000端口映射容器中的9000端口
-v /var/run/docker.sock:/var/run/docker.sock     # 把宿主机的Docker守护进程(docker daemon)默认监听的Unix域套接字挂载到容器中
-v /root/portainer:/data    # 把宿主机目录 /root/portainer 挂载到容器 /data 目录；
--name portainer # 指定运行容器的名称
```
```Linux
# firewall-cmd --permanent --add-port=8000/tcp  // 永久添加端口
# firewall-cmd --reload
# firewall-cmd --list-all
```
# 4.portainer 管理
portainer安装注册界面
![portainer安装注册](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/%E9%9D%A2%E6%9D%BF%E5%AE%89%E8%A3%85%E7%95%8C%E9%9D%A2.PNG)
选择访问Local管理本地docker
![本地docker安装管理](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/%E6%9C%AC%E5%9C%B0docker%E7%AE%A1%E7%90%86.PNG)
portainer 管理界面
![portainer 界面](https://hfm-wp.oss-cn-hangzhou.aliyuncs.com/portainer%20%E9%9D%A2%E6%9D%BF/%E9%9D%A2%E6%9D%BF%E7%95%8C%E9%9D%A2.PNG)