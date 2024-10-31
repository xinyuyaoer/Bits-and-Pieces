# 基于阿里云ECS与ACR的python微服务镜像构建、部署与接口访问实验

## 实验目的

* 熟练掌握使用Dockerfile构建容器镜像的基本方法.
* 熟练掌握阿里云云服务器ECS与容器镜像服务ACR的相关操作.
* 理解HTTP协议基本概念,熟练掌握HTTP调试工具的使用.
* 理解网络服务API的基本概念、分类, 重点掌握HTTP RESful API的定义原则、应用模式.
* 理解并掌握基于python Requests 库撰写代码访问HTTP RESful API定义原则、应用模式.

## 实验要求

本实验以设备管理服务中设备实体的增删改查接口为例, 提供采用python Sanic 撰写的微服务范例(使用`Pyinstaller` 打包为可执行程序, 不直接提供代码). 要求学生在学习微服务接口定义与相关数据结构的基础上完成以下具体任务:

* 在本地开发环境启动、测试微服务范例
* 在本地开发环境创建Dockerfile, 将微服务范例程序打包成基础镜像, 并上传至阿里云容器仓库.
* SSH登陆阿里云ECS, 从ECS登陆阿里云容器镜像ACR, 拉取承载微服务范例程序的镜像并部署应用.
* 基于ECS公网IP访问微服务范例程序接口, 查阅四个范例接口的API文档, 随后使用python Request 库编码访问微服务范例接口, 体验HTTP RESTful API接口应用模式与能力。

## 实验环境

* 本地开发环境: Python3、Python Requests
* 阿里云云服务器ECS(Ubuntu Server): Docker
* 阿里云容器镜像服务(ARC)
* Python微服务范例: