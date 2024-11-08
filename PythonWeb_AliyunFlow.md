# 基于阿里云云效Flow的python Web服务框架与部署实验

## 实验目的

* 理解并掌握 Python 虚拟环境的应用价值与管理操作。
* 熟练掌握 Docker 命令与 Dockerfile 构建逻辑，能够基于 Dockerfile 创建容器镜像。
* 熟练掌握阿里云云服务器 ECS、阿里云云效代码管理 Codeup 与流水线 Flow，以及阿里云容器镜像服务ACR的相关配置与管理操作。
* 理解并掌握 Python 项目的多种部署方式，初步体验流水线的应用模式。

## 实验要求

本实验提供一个简易的 Python Web 服务范例。该服务基于Python Sanic框架构建实现了设备基本信息的可视化展示。要求学生下载代码后跟随实验步骤完成下实验内容: 

* 首先，分别以「虚拟环境(venv)」形式与容器形式在阿里云云服务器 ECS 上直接部署范例服务。
* 随后，基于阿里云云效流水线 Flow，分别以「虚拟环境(venv)」形式与容器形式部署范例服务，初步体验流水线在 DevOps 持续集成/持续部署(CI/CD)中的重要租作用。

通过以上实验内容，综合应用阿里云云服务器 ECS、云效代码管理 Codeup、云效流水线 Flow，初步搭建体验DevOps CI/CD 环境.

## 实验环境

* 本地实验环境: Git, python3, Python Venv, Sanic
* 阿里云云服务ECS: docker, python3, Python Venv, Sanic
* 阿里云云效: 代码管理Codeup, 流水线Flow
* 阿里云容器镜像服务(ACR)
* Python Web服务范例`exp_pyms_demo.tar.gz`

## 实验步骤

### 初始范例代码

* 服务范例`exp_pyms_demo` 文件组织结构

```Plain Text
.
├── devices.csv           # 存放设备信息的 csv 文件
├── requirements.txt      # 运行所需安装的 Python 库
├── server.py             # Python Sanic 服务执行文件，服务使用8000端口启动
└── static                # 静态文件夹，包含 html, js 和 css 文件
    ├── index.html        # 网页页面，URL 为 <ip>:8000/
    ├── index.js          # JavaScript 文件
    └── style.css         # 样式表文件

1 directory, 6 files
```

### 构建实验环境

#### 准备本地开发环境

##### 在本地环境安装Python第三方库

在本地工作目录 `exp_pyms_demo` (先创建本地工作文件夹`exp_pyms_demo` , 然后`cd`进入)创建 Python 虚拟环境 `pyms_venv` ，使用`source` 命令激活虚拟环境。

Python 虚拟环境可以实现为不同的项目设置独立的依赖库，以实现在统一系统中不同项目所需软件包的隔离。例如开发环境中的两个项目都依赖同一个软件包，但依赖于不同的版本，或者一些依赖是相互冲突的，那么使用 Python 虚拟环境就能为不同的项目创建独立的虚拟环境。

```Plain Text
python3 -m venv pyms_venv # 创建虚拟环境
source pyms_venv/bin/activate # 激活环境
```

配置 `requirements.txt` 文件，提供要安装的 Python 库名，本实验仅需 Python Web 服务框架 `Sanic`。

根据 `requirements.txt` 文件安装第三方库。

输入以下指令

```Plain Text
touch requirements.txt
```
打开`requirements.txt` 写入一下内容:

```Plain Text
sanic==20.12.4
```
然后执行以下指令:

```Plain Text
pip install -r requirements.txt
```
![environment](exp4_picture\image.png)

#### 准备云服务器ECS
(已实现)

#### 在ECS安装docker引擎和python3
(已实现)

#### 基于阿里云容器镜像服务ACR配置镜像仓库
(已实现)

#### 基于阿里云云效代码管理Codeup创建代码库
* 在Codeup新建远程仓库
   * 访问 [云效代码管理 Codeup](https://www.aliyun.com/product/yunxiao/codeup) 产品首页，登陆后若首次使用，则需创建或者加入一个企业。
   * 新建代码库，命名为 `exp_pyms_demo`，新建时请注意勾选「创建.gitignore], 并选择Python模版。

   * 新建后，Codeup 默认创建 Master 分支并提供推荐 .gitignore 文件。

* 配置SSH访问Codeup远程仓库
   * 根据 [如何配置SSH密钥及自定义SSH认证密钥的路径-云效-阿里云帮助中心](https://help.aliyun.com/zh/yunxiao/user-guide/configure-ssh-key), 分别在本地环境与 ECS 服务器上创建 SSH 公钥私钥，并在 Codeup 对应页面添加公钥，保证本地环境与 ECS 服务器均可基于 SSH 访问 Codeup 远程仓库。
本地环境在之前的实验已经配置了SSH，现在只需要在ECS上配置SSH即可。
![ECSssh](exp4_picture\image-1.png)
![codeupssh](exp4_picture\image-2.png)

### 基于阿里云云服务器ECS直接部署范例服务

以下操作均在阿里云ECS服务器上进行

#### 获取范例服务包并解压

如下图所示，利用ssh命令将`exp_pyms_demo.tar.gz`文件从github远程仓库下载到ECS服务器上。
![demo](exp4_picture\image-3.png)

#### 在虚拟环境部署范例服务

创建Python虚拟环境`venv`并安装`requirements.txt` 指定的相关依赖:

```Plain Text
cat requirements.txt # 查看所需依赖
python3 -m venv exp_venv # 创建虚拟环境
source exp_venv/bin/activate # 激活虚拟环境
pip3 install -r requirements.txt # 安装所需依赖
```
![venv](exp4_picture\image-4.png)

启动Python Web服务:

```Plain Text
python3 server.py
```

😵‍💫遇到`UnboundLocalError: local variable 'worker_state' referenced before assignment` 问题, 这有可能是8000端口被占用了

:bulb:**解决方案**
你可以通过以下命令查找占用端口 8000 的进程，并终止它：

```Plain Text
sudo lsof -i :8000
sudo docker ps
sudo docker stop <容器ID>
```
然后重复前面的命令，成功安装sanic。
![sanic](exp4_picture\image-5.png)

选择某台能访问 ECS IP 的终端设备，使用浏览器访问`<IP>:8000/`，查看服务页面输出。

```Plain Text
# 查看公网ip的指令
curl ifconfig.me
```
![:8000](exp4_picture\image-6.png)
查看无误后，键入`ctrl+c`停止服务。随后退出虚拟环境`venv`，删除虚拟目录

```Plain Text
deactivate # 退出虚拟环境
rm -r exp_venv # 删除
```

#### 以容器形式部署范例服务

在 `exp_pyms_demo` 目录下创建 `Dockerfile` 文件，将 Python Web 服务范例代码打包进容器镜像。

```Plain Text
# 创建Dockerfile文件
touch Dockerfile

# 编写Dockerfile文件
nano Dockerfile
```

在 `Dockerfile` 文件中，编写如下内容:
```docker
# 基于 Python 最新基础镜像
FROM python:3.10-slim-buster
LABEL maintainer="<hyx_beryl@qq.com>"

# 工作目录
WORKDIR /app

# 安装python库
COPY requirements.txt requirements.txt
RUN apt-get update && apt-get install -y \
    && pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

# 清理 apt 软件包缓存
RUN rm -rf /var/lib/apt/lists/*

# 复制当前文件夹中全部文件到镜像中
COPY . .

# 暴露服务端口 8000
EXPOSE 8000

# 容器启动时执行命令
CMD ["python3", "server.py"]
```
使用 `docker build` 命令构建镜像，完成后通过 `docker images` 可查询.

```docker
docker build -t exp_pyws_demo .
docker images
```
![build](exp4_picture\image-7.png)
![images](exp4_picture\image-8.png)
基于镜像`exp_pyws_demo` 启动容器:

```docker
docker run -d --restart=always --name beryl_exp_pyws_demo -p 8000:8000 exp_pyws_demo
```
![image](exp4_picture\image-9.png)
选择一台终端设备，使用浏览器访问访问 ECS 公网IP URL `http://<IP>:8088/`查看服务页面输出。此外也可在 ECS 中使用`curl` 命令访问 ECS 本地服务 URL，观察输出以确认服务部署是否生效。

```docker
curl http://127.0.0.1:8000
```
![curl](exp4_picture\image-11.png)
![devices](exp4_picture\image-10.png)
测试确认部署无误，关闭容器，删除容器与镜像。
```Plain Text
docker stop exp_pyws_demo
docker rm exp_pyws_demo
docker rmi exp_pyws_demo
```
## 基于阿里云云效Flow在虚拟环境部署范例服务

#### 配置Git仓库, 创建分支venv

先创建一个工作文件夹`exp_flow`,然后进行git拉取**自己放在Codeup远程仓库上的的项目**, 将范例服务包下载到本地环境(继续使用之前已经搭建好的虚拟机)后，解压进入目录exp\_pyms\_demo ，将该目录作为Git工作目录。此时查看目录，内容如下:

```docker
git clone git@codeup.axxxxxxxxxxxxxxxxxxxxxxxf371e/exp_pyms_demo.git
ls # 查看
cd exp_pyms_demo
tar -xzvf exp_pyms_demo.tar.gz
cd exp_pyws_demo
ls -a -p
```
![venv](exp4_picture\image-12.png)
做到这一步我们已经和远程仓库`exp_pyms_demo`建立关联了

查看当前所在分支与工作目录文件状态。每次开工之前，首先从远程仓库 `git pull`

```Plain Text
git status
git pull origin master
ls -a -F
```
![pull](exp4_picture\image-13.png)
通过 `git status` 观察工作目录文件状态，随后全部纳入跟踪且暂存，再提交。

```Plain Text
git status # 检查当前仓库状态

git add . # 将所有文件加入暂存区（追踪文件）
git commit -m"init" # 提交改动，添加提交信息 "init"

git push -u origin master # 推送当前分支到远程仓库的 master 分支，并与远程仓库关联

git checkout -b venv # 创建并切换到一个名为 venv 的新分支
git status
git push --set-upstream origin venv # 将 venv 分支推送到远程仓库，并与远程的 venv 分支建立追踪关系
```
![add](exp4_picture\image-14.png)
![push](exp4_picture\image-15.png)

#### 配置云效Flow以支持ECS虚拟环境部署

##### 根据模版创建流水线

在云效流水线 Flow 系统新建流水线，选择云效预置模版Python·测试、构建、部署到阿里云ECS/自有主机」。将「测试」、「构建」、「部署」三个阶段命名分别改为「代码测试」、「制品构建」以及「服务部署」。

![flow](exp4_picture\image-16.png)

##### 配置阶段[代码测试]

本阶段模版设置有「Python 单元测试」与「Python 代码扫描」两个任务。因本实验暂不考虑单元测试，故**删除「Python 单元测试」任务**。而 Python 代码扫描 任务保持模版默认配置即可。

##### 配置阶段[制品构建]

本阶段模版设置有一个任务「Python 构建上传到仓库」，包含「Python 构建」与「构建物上传」两个步骤。本实验无需对 Python 脚本进行特殊构建，故只需使用默认设置将代码制品打包上传至阿里云提供的「云效公共存储空间」即可。

##### 配置阶段[服务部署]

本阶段模版设置有一个任务「主机部署」，其中

* 「制品」选择上一步「构建物上传」步骤设定的制品名称`Artifacts_${PIPELINE_ID}`

* 「主机组」
   * 首次使用时，需要点击「新建主机组」，主机名设置`beryl_exp_flow`「添加新主机」弹出框中选择「阿里云ECS」。
    ![host](exp4_picture\image-17.png)

   * 添加服务器连接
    添加方式选择「阿里云ECS」，点击「添加服务连接」后如图操作。
    ![link](exp4_picture\image-18.png)

   * 选择地域, 并选择实例进行创建
    ![region](exp4_picture\image-19.png)

* 配置部署
   * 「下载路径」保持默认即 `/home/admin/app/package.tgz`
   * 「执行用户」保持默认即 `root`。
   * 「部署脚本」定义如下:

```Plain Text
# 清理上一次部署时的运行环境
pkill python3
rm -rf ~/exp_pyws_demo

# 解压代码制品到指定运行目录
mkdir ~/exp_pyws_demo
tar -xvzf /home/admin/app/package.tgz -C ~/exp_pyws_demo 
# 特别注意，此处路径必须与「部署配置->下载路径」一致
# 特别注意2, -C 后面的路径一定是你在ECS上对应的exp_pyws_demo路径
cd ~/exp_pyws_demo/exp_pyws_demo
# 特别注意3, 路径一定是你在ECS上对应的exp_pyws_demo路径

# 建立 Python 虚拟环境并安装依赖
python3 -m venv exp_venv
source exp_venv/bin/activate
pip3 install -r requirements.txt

# 运行 Web 服务
nohup python3 server.py > /dev/null 2>&1 &
# nohup: 忽略挂起信号，终端退出不会影响运行。
# > /dev/null: 将 server.py 的日志输出重定向到 /dev/null，不显示在终端。
# 2>&1: 将标准错误输出也重定向到标准输出。
# &: 让程序在后台运行，不会因为终端退出而中断。
```

#### 运行流水线

点击「保存并运行」按钮，在弹出的对话框中点击「运行」按钮，流水线即被触发逐一执行阶段任务直至最终完成服务部署。
![run](exp4_picture\image-20.png)

此时阿里云 ECS 已经基于容器启动了 Python web 服务，通过 curl 命令可测试服务接口是否可访问。

```Plain Text
curl http://127.0.0.1:8000
```
![curl](exp4_picture\image-21.png)
同样也可以选择某台能访问 ECS IP 的终端设备，使用浏览器访问`<IP>:8000/`，可查看服务页面输出。

#### 修改设别文件, 提交代码并触发流水线

在本地环境(Ubuntu)修改`devices.csv` , 添加一条设备信息, 保存并退出

```Plain Text
nano devices.csv
```

```Plain Text
# 添加以下内容
7097988787878778889,device4,client,PC,00003,10,2023-09-01 00:00:00,WiFi,00:00:00:00:00:16,156.123.3.14,alive
```

直接提交并推送远程仓库并触发流水线执行。

```Plain Text
git commit -a -m "docs: Add a device info in devices.csv"
git push origin venv
```
![ubuntuDevices](exp4_picture\image-23.png)
![devices](exp4_picture\image-22.png)
流水线自动执行完毕后，使用浏览器重新访问该服务，可发现设备信息表多出一行，更新成功。
![web](exp4_picture\image-24.png)

#### 完成测试后关闭范例服务

```Plain Text
ps aux | grep server.py # 查看正在运行的 server.py 进程, 找到root目录下进程号 (PID) 
sudo kill 进程号 (PID) 终止该进程： 使用 kill 命令终止进程号 (你刚刚找到的)的 server.py 进程
ps aux | grep server.py # 再次检查 server.py 是否仍在运行：
```
![kill](exp4_picture\image-25.png)

### 基于阿里云云效 Flow 以容器形式部署范例服务

#### 配置Git仓库, 创建分支`docker`

在本地环境从当前 `venv` 分支中创建检出 `docker` 分支。

```Plain Text
git checkout -b docker
git branch
```

准备 `Dockerfie` 文件，用于在流水线中制作承载范例服务的 Docker 镜像。

```Plain Text
nano Dockerfile
```

在 `Dockerfile` 文件中，编写如下内容:

```Plain Text
# 基于 Python 最新基础镜像，从 https://docker.1panel.live 拉取
FROM docker.1panel.live/library/python:3.10-slim-buster
LABEL maintainer="<ljh2629827042@qq.com>"

# 工作目录
WORKDIR /app

# 安装python库
COPY requirements.txt requirements.txt
RUN apt-get update && apt-get install -y \
    && pip3 install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

# 清理 apt 软件包缓存
RUN rm -rf /var/lib/apt/lists/*

# 复制当前文件夹中全部文件到镜像中
COPY . .

# 暴露服务端口 8080
EXPOSE 8080

# 容器启动时执行命令
CMD ["python3", "server.py"]
```

注意暴露的服务端口要和 `server.py` 中一致。
然后执行下面的指令：

```Plain Text
git add .
git commit -m "docs: Add Dockerfile"
git push --set-upstream origin docker # 推送 docker 分支到远程仓库并设置追踪关系
```
![docker](exp4_picture\image-26.png)

#### 配置云效Flow以支持ECS容器镜像部署

基于前面配置的云效 Flow 流水线进行修改，以支持 ECS 容器镜像模式部署范例服务。即，在云效控制台页面选中之前配置的流水线，点击「编辑」按钮，进入流水线配置页面。其中「代码测试」阶段保持不变，其他三个阶段均需要修改。

##### 配置阶段「流水线源」

将「默认分支」修改为「docker」分支。
![flow source](exp4_picture\image-27.png)

##### 配置阶段「镜像构建」

将原「制品构建」阶段更名为「镜像构建」，首先新建「并行任务」用于构建镜像随后再删除原有任务「Python 构建上传到仓库」(否则若该阶段没有任务存在，自身也会被自动删除)

新建「并行任务」时，在弹出页面中选择「镜像构建」，再选择「Python 镜像构建」


随后配置任务「Python 镜像构建」，在步骤「镜像构建并推送至阿里云镜像仓库个人版」中:

* 点击「添加服务链接」，根据页面提示创建新的服务链接，「使用范围」设为私密。
* 选择「地域」，如自己的ACR在哪就在哪, 我的在西南1(成都)
* 点击「仓库」旁的 `+`按钮，根据页面提示「新建」仓库，命名空间建议设定为「exp_\<name>」(因该命名空间为全局命名空间，故而必须唯一)，仓库名称则统一定义为「exp\_pyms\_demo」。
* 其他配置保持默认即可，其中标签:默认为 `${DATETIME}`; Dockerfile路径:默认为 Dockerfile即从仓库目录中直接查找

最后删除「Python 构建上传到仓库」
![image](exp4_picture\image-28.png)

##### 配置阶段「服务部署」

首先新建「并行任务」用于容器部署，随后再删除原有任务「主机部署」(否则若该阶段没有任务存在，自身也会被自动删除)

新建「并行任务」时，在弹出页面中选择「部署」，再选择「Docker部署」。

在具体配置「Docker部署」任务之前，先点击页面顶部的「变量和缓存」，新建字，必须采符变量 `ACR_LOGIN_PWD` ，默认值为登陆阿里云容器镜像服务 ACR 时所需密码，用「私密模式」保存。该变量将用于后续任务配置中的「部署脚本」。
![pwd](exp4_picture\image-30.png)

完成「变量和缓存」的操作后，回到「流程配置」页面，在「服务部署」阶段配置「Docker部署」任务:

* 「主机组」选择刚刚所建主机组
* 「部署配置」
   * 「执行用户」保持默认即 root。
   * 「部署脚本」定义如下，使用时请更换阿里云账号 username 以及地域region 。(事实上，建议将username、region(或者整个 URL)、容器名称都设置为Shell 变量，以增加脚本的通用性，请自行尝试。)

```Plain Text
docker login --username=<你的名字> --password=${ACR_LOGIN_PWD} registry.cxxxxxxx.aliyuncs.com (可以在ACR中直接复制)

docker rm -f exp_pyms_demo  # 确保历史容器实例

docker pull ${CONTAINER_TAG}
docker run -d --restart=always --name exp_pyms_demo -p 8080:8080 ${CONTAINER_TAG}
```

* 「变量」
   * 点击「添加」新增变量，选择「上游任务 制品/镜像 下载地址」，新增变量「CONTAINER\_TAG」，变量值选择 标签`.${DATATIME}`，与上一阶段任务「Python 镜像构建」产出的镜像标签配置保持一致。
![docker](exp4_picture\image-29.png)

#### 运行流水线

点击「保存并运行」按钮，在弹出的对话框中点击「运行」按钮，流水线即被触发逐一执行阶段任务直至最终完成服务部署。
![runflow](exp4_picture\image-31.png)

运行完成后，在阿里云 ECS 上执行 docker images与 docker ps 命令查看到生成的镜像和容器。此时使用浏览器重新访问该服务，可见如前面的页面输出。
![ps](exp4_picture\image-32.png)

#### 修改代码文件, 提交代码并触发流水线

观察刚刚运行完流水线之后，在本地环境根据「代码测试」阶段「Python 代码扫描」是存在问题的，根据上次执行所提示的信息修订 server.py 存在的格式问题后提交、推送。
接下来修改server.py文件, 提交并推送远程仓库并触发流水线执行。
执行完发现结果有变化，实验成功。
![amend](exp4_picture\image-33.png)


