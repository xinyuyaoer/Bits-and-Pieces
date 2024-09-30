# 基于阿里云云效Codeup的Git代码管理

## 实验环境

Ubuntu 20版本

## 实验内容及步骤

### 构建实验环境

#### 准备本地开发环境

##### 在本地环境安装Git和python3

在终端输入指令安装Git并检查是否安装成功：
![git1](git_install.png)
![git2](git_version.png)

配置全局用户名与邮箱。为标识身份，在使用 Git 前，建议执行两个全局配置，即用户名与电子邮箱。当使用 Git 提交代码时，用户名和电子邮箱将被同时记录，用以标识提交者信息，推送时亦然。以本地环境Ubuntu为例，若未配置 Git 用户名与电子邮箱，提交代码后Git 记录的提交者为当前 Ubuntu 本地用户名。

```
git config --global user.name "<username>"
git config --global user.email "<email>"
```

查看配置情况，输入以下指令：

```
git config -global user.name
git config --global user.email
```
![git3](配置用户名邮箱.png)

随后安装Python3和Python虚拟环境，输入以下指令：
```
sudo apt install python3
sudo apt install python3-venv
```

![alt text](python_install.png)

接着查看Python版本，输入以下指令：
```
python --version
```

![alt text](python3_.png)

##### 在本地安装Python第三方库
