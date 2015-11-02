
# WampServer+Composer+laravel5+Git
# 打造本地PHP+mysql开发环境

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

执行命令下载安装 Laravel5.0.22，
这里安装的是5.0.22，所以我写的安装目录为laravel5。

输入本地地址测试是否安装成功：
http://localhost/laravel/public/
http://localhost/laravel/public/index.php/home

## 3、以上安装成功后，我们可能要建立多个虚拟主机。
为何要建立多个虚拟主机呢？有时我们可以下载多个开源框架或者程序来研究哪些框架或程序要好。
比如：
host1.emao.com 研究 host1的程序
host2.emao.com 研究 host2的程序

## 进入 E:\wamp\bin\apache\apache2.4.9\conf ，打开 httpd.conf 找到 # Virtual hosts 并修改为：

# Virtual hosts
Include conf/extra/httpd-vhosts.conf

## 进入 E:\wamp\bin\apache\apache2.4.9\conf\extra ，打开 httpd-vhosts.conf 添加：

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

Alias /laravel5 "E:\wamp\www\laravel5\public"

<Directory "E:\wamp\www\laravel5\public">
AllowOverride All
Order Deny,Allow
Allow from all
</Directory>

<VirtualHost *:80>
ServerName laravel5.emao.com
ServerAlias laravel5.emao.com
DocumentRoot "E:\wamp\www\laravel5\public"
</VirtualHost>

这里分别为权限配置、局域网指向、本地指向和laravel5的测试指向，这样做是为了更好地分开做不同的研究。

## 4、启用rewrite module

主要是让URL更短更易记，如：
http://laravel5.emao.com/index.php/auth/login
rewrite为：
http://laravel5.emao.com/auth/login

## 步骤

按照上面的httpd-vhosts.conf配置后，需要启用wamp的rewrite module，如图：

![](http://blog.cmstutorials.org/wp-content/uploads/2009/11/activate_rewrite_module1.jpg)

激活后，修改 E:\wamp\www\laravel5\public 下的 .htaccess 文件为：

<IfModule mod_rewrite.c>
<IfModule mod_negotiation.c>
Options -MultiViews
</IfModule>

RewriteEngine On
RewriteBase /laravel5/

# Redirect Trailing Slashes If Not A Folder...
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)/$ /$1 [L,R=301]

# Handle Front Controller...
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
</IfModule>

重启下wamp，试着使用 http://laravel5.emao.com/auth/login 访问，能访问表示配置成功。

## 5、创建数据库

# 点击wamp托盘图标进入phpmyadmin 的 http://localhost/phpmyadmin/，创建一个MYSQL数据库用户和数据库。

# 进入E:\wamp\www\laravel5\config\database.php 配置数据库，如：

		'mysql' => [
			'driver'    => 'mysql',
			'host'      => env('DB_HOST', 'localhost'),
			'database'  => env('DB_DATABASE', 'laravel'),
			'username'  => env('DB_USERNAME', 'laravel'),
			'password'  => env('DB_PASSWORD', 'laravel12345'),
			'charset'   => 'utf8',
			'collation' => 'utf8_unicode_ci',
			'prefix'    => '',
			'strict'    => false,
		],


另外，如果要修改数据库配置信息，你还需要修改laravel目录下.env文件修改对应的配置参数：

DB_HOST=localhost
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=root

## 5.1、创建示例数据库

下面我们来试着创建，

Laravel 已经为我们准备好了 Auth 部分的 migration，运行以下命令执行数据库迁移操作：

php artisan migrate

我们打开数据库

至此，数据库迁移已完成，你可以打开 http://laravel5.emao.com/auth/login 进行注册和登录。

## 6、安装Git

以上我们完成了PHP本地环境的搭建，你可以开始学习Laravel了，当然，你也可以学习别的PHP框架，很难说哪个最好，重要的是简单易懂、组件丰富、高效开发、文档完善，总之适合自己团队的才是关键，或者有一天你的团队庞大到不需要框架了，而且更注重项目的商业隐私了。

下面我们来说说Git，Git的安装是为了代码托管，也就是所谓的开源，开源了别人也可以帮你检查和改进代码，当然隐私或涉及机密的就别Git上去了，比如你的银行卡和银行卡密码，Github也有私有托管，不过是要花银子的。

怎样安装大家可以看线上的教程了，这个比较简单。

# 推荐教程：git的简明指南
http://rogerdudler.github.io/git-guide/index.zh.html

# 一些常用的git命令
git config --list
git clone https://github.com/romy2012/emao.git
git status
git add *
git commit -m "本地文件搭建测试"
git push origin master

#  Git使用中相关涉及到的解决方案

# Git Push 避免重复输入用户名和密码方法

在大家使用github的过程中，一定会碰到这样一种情况，就是每次要push 和pull时总是要输入github的账号和密码，这样不仅浪费了大量的时间且降低了工作效率。在此背景下，本文在网上找了两种方法来避免这种状况，这些成果也是先人提出来的，在此只是做个总结。

## 方法一、 

## 1.1、创建文件存储GIT用户名和密码

在%HOME%目录中，一般为C:\users\Administrator，也可以是你自己创建的系统用户名目录，反正都在C:\users\中。文件名为.git-credentials,由于在Window中不允许直接创建以".“开头的文件，所以需要借助git bash进行，打开git bash客户端，进行%HOME%目录，然后用touch创建文件 .git-credentials, 用vim编辑此文件：

touch .git-credentials
vim .git-credentials

输入内容格式：

https://{username}:{password}@github.com

## 1.2、添加Git Config 内容

进入git bash终端，输入如下命令：

git config --global credential.helper store

执行完后查看%HOME%目录下的.gitconfig文件，会多了一项：

[credential]
    helper = store
    
重新开启git bash，会发现git push时不用再输入用户名和密码。

## 方法二、

## 2.1、添加环境变量

在windows中添加一个HOME环境变量，变量名:HOME,变量值：%USERPROFILE%

## 2.2、创建git用户名和密码存储文件

进入%HOME%目录，新建一个名为"_netrc"的文件，文件中内容格式如下：

machine {git account name}.github.com
login your-usernmae
password your-password

重新打开git bash即可，无需再输入用户名和密码。
