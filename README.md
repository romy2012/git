# 1、下载并安装 WampServer

## 执行命令
    php --version 
测试是否安装成功

    C:\Users\pc>php --version
    PHP 5.5.12 (cli) (built: Apr 30 2014 11:20:58)
    Copyright (c) 1997-2014 The PHP Group
    Zend Engine v2.5.0, Copyright (c) 1998-2014 Zend Technologies
        with Xdebug v2.2.5, Copyright (c) 2002-2014, by Derick Rethans

# 2、安装 Composer
## 执行命令
    Composer --version
测试是否安装成功
    C:\Users\pc>Composer --version
    Composer version 1.0-dev (c557715669ba8dd2dc6c63859f919351a4aa5e2f) 2015-10-28 14:13:03

更新Composer到最新版本
    composer self-update

## 2.1、 进入到 E:\wamp\www 目录 右键> Use Composer here
    composer create-project laravel/laravel laravel5 5.0.22
执行命令下载安装 Laravel5.0.22
这里安装的是5.0.22，安装目录为laravel5

输入本地地址测试是否安装成功
http://localhost/laravel/public/
http://localhost/laravel/public/index.php/home

## 以上安装成功后，我们可能要建立多个虚拟主机
为何要建立多个虚拟主机呢？
有时我们可以下载多个开源框架或者程序来研究哪些框架或程序要好。
比如：
host1.emao.com 研究 host1的程序
host2.emao.com 研究 host2的程序

## 进入 E:\wamp\bin\apache\apache2.4.9\conf
打开 httpd.conf 找到 # Virtual hosts 并修改为
    # Virtual hosts
    Include conf/extra/httpd-vhosts.conf

## 进入 E:\wamp\bin\apache\apache2.4.9\conf\extra
打开 httpd-vhosts.conf 添加

<Directory "E:\wamp\www">
AllowOverride All
Order Deny,Allow
Allow from all
</Directory>

<VirtualHost *:80>
ServerName 192.168.1.78
DocumentRoot "E:\wamp\www"
</VirtualHost>

<VirtualHost *:80>
ServerName hosts.emao.com
DocumentRoot "E:\wamp\www"
</VirtualHost>

<VirtualHost *:80>
ServerName laravel5.emao.com
DocumentRoot "E:\wamp\www\laravel5\public"
</VirtualHost>

这里分别为权限配置、局域网指向、本地指向和laravel5的测试指向，这样做是为了更好地分开做不同的研究。





# Git Push 避免重复输入用户名和密码方法
## 前言

## 在大家使用github的过程中，一定会碰到这样一种情况，就是每次要push 和pull时总是要输入github的账号和密码，这样不仅浪费了大量的时间且降低了工作效率。在此背景下，本文在网上找了两种方法来避免这种状况，这些成果也是先人提出来的，在此只是做个总结。

## 1.方法一 

## 1.1 创建文件存储GIT用户名和密码

## 在%HOME%目录中，一般为C:\users\Administrator，也可以是你自己创建的系统用户名目录，反正都在C:\users\中。
## 文件名为.git-credentials,由于在Window中不允许直接创建以"."开头的文件，所以需要借助git bash进行，打开git bash客户端，进行%HOME%目录，然后用touch创建文件 .git-credentials, 用vim编辑此文件，输入内容格式：

    touch .git-credentials

    vim .git-credentials

    https://{username}:{password}@github.com

## 1.2 添加Git Config 内容

## 进入git bash终端， 输入如下命令：

    git config --global credential.helper store

## 执行完后查看%HOME%目录下的.gitconfig文件，会多了一项：

    [credential]

    helper = store
重新开启git bash会发现git push时不用再输入用户名和密码


## 2.方法二
## 2.1 添加环境变量

## 在windows中添加一个HOME环境变量，变量名:HOME,变量值：%USERPROFILE%

## 2.2 创建git用户名和密码存储文件

## 进入%HOME%目录，新建一个名为"_netrc"的文件，文件中内容格式如下：

    machine {git account name}.github.com
    login your-usernmae
    password your-password

## 重新打开git bash即可，无需再输入用户名和密码
