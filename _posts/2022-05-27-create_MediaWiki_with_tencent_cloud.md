---
layout: post
title: "用腾讯轻量云搭建MediaWiki百科"
date:   2022-05-27
tags: [wiki,生物信息]
comments: true
author: Si Yangming
toc : true
---

[MediaWiki](https://blog.idc.moe/go/aHR0cHM6Ly93d3cubWVkaWF3aWtpLm9yZy93aWtpL01lZGlhV2lraQ) 是 Wikipedia 以及世界各地社区和公司部署的许多 wiki 背后的软件。 它常常被大公司用来做文档和百科全书。 本篇文章将教学如何在腾讯云轻量应用服务器上安装 MediaWiki。

首先需要一台[腾讯轻量应用服务器](https://blog.idc.moe/go/aHR0cHM6Ly9jdXJsLnFjbG91ZC5jb20vUE1SU3JBaXo) 购买：[https://curl.qcloud.com/PMRSrAiz](https://blog.idc.moe/go/aHR0cHM6Ly9jdXJsLnFjbG91ZC5jb20vUE1SU3JBaXo)

以 CentOS 8 Stream为例，不依赖任何面板。

# 安装 Web 服务

1. 以Apache 2.4 为例:

   ```shell
   sudo yum install httpd
   ```

2. 启动 Apache 服务器:

   ```shell
   sudo systemctl enable httpd.service
   sudo systemctl start httpd.service
   ```

# 安装 PHP 环境

MediaWiki 需要 PHP 7.3.19–24、7.4.3 或更高版本。 但是，CentOS 软件包管理器 (Yum) 仅在其默认存储库中包含 PHP 7.2。 因此，您需要使用 Remi 存储库来获取较新的 PHP 版本之一。

1. 为 Enterprise Linux (EPEL) 和 Remi 存储库添加额外的包：

```shell
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

2. 从 Remi 存储库安装 PHP：

```shell
sudo dnf module reset php
sudo dnf module install php:remi-8.0
```

使用了最新版本的 PHP 8.0。

3. 安装 `php-mysqlnd` 模块以支持 MariaDB 的使用（如下所述）：

```shell
sudo dnf install php-mysqlnd
```

4. 重启 Apache 服务：

```shell
sudo systemctl restart httpd
```

# 安装&配置 MariaDB 数据库

MediaWiki 支持多种数据库选项，包括 MariaDB、MySQL 和 PostgreSQL。 MariaDB 在 MediaWiki 文档中是首选，因此在我们将用 MariaDB 作为例子

1. 安装 MariaDB:

   ```shell
   sudo yum install mariadb-server
   ```

2. 启动 MariaDB 服务:

   ```shell
   sudo systemctl enable mariadb
   sudo systemctl start mariadb
   ```

3. 安全安装 MariaDB:

   ```
   sudo mysql_secure_installation
   ```

   该脚本让您可以选择更改 MariaDB 的root密码、删除匿名用户帐户、禁用 localhost 之外的根登录以及删除测试数据库。 建议您对这些选项回答“是”。 您可以在 [MariaDB 知识库](https://blog.idc.moe/go/aHR0cHM6Ly9tYXJpYWRiLmNvbS9rYi9lbi9tYXJpYWRiL215c3FsX3NlY3VyZV9pbnN0YWxsYXRpb24v) 中阅读有关该脚本的更多信息。

4. 通过以 root 用户身份打开 MariaDB (`sudo mysql -u root -p`) 并输入以下示例中给出的命令，为 MediaWiki 创建一个数据库和一个数据库用户。 将 Bioinfor_wiki 替换为所需的数据库名称，将 `admin` 替换为所需的数据库用户名，并将 `password` 替换为该用户的密码，该密码不应与数据库的 root 密码匹配：

   ```shell
   sudo mysql -u root -p (2120)
   CREATE DATABASE Bioinfor_wiki;
   CREATE USER 'admin'@'localhost' IDENTIFIED BY 'password';
   CREATE USER 'alphaUser'@'localhost' IDENTIFIED BY '';
   
   GRANT ALL PRIVILEGES ON Bioinfor_wiki.* TO 'admin'@'localhost' WITH GRANT OPTION;
   ```

5. 然后退出 MariaDB：

   ```
   exit;
   ```

# 下载&提取 MediaWiki 文件

1. 从 [官方 MediaWiki 下载页面](https://blog.idc.moe/go/aHR0cHM6Ly93d3cubWVkaWF3aWtpLm9yZy93aWtpL0Rvd25sb2Fk) 下载包含最新版本 MediaWiki 软件的 `tar.gz`。

   或用下面指令获取：

   ```
   sudo yum install wget
   wget -c https://releases.wikimedia.org/mediawiki/1.37/mediawiki-1.37.2.tar.gz
   ```

2. 将 `tar.gz` 文件移动到 Apache Web 服务器的文档目录。 您可以在 Apache 配置文件中的 `DocumentRoot` 变量中找到文档目录，位于`/etc/httpd/conf/httpd.conf`； 典型的文档目录是`/var/www/html`，在以下示例中假定：

   ```
   sudo mv mediawiki-1.37.2.tar.gz /var/www/html
   ```

3. 导航到文档目录，并提取归档文件：

   ```
   cd /var/www/html/
   sudo yum install tar
   sudo tar xvzf /var/www/html/mediawiki-1.37.2.tar.gz
   ```

   建议您重命名生成的文件夹，因为文件夹名称成为用于导航到 MediaWiki 的 URL 的一部分。 对于本指南的其余部分，名称 `w`iki 用于此文件夹：

   ```
   sudo mv /var/www/html/mediawiki-1.37.2 /var/www/html/wiki
   ```

# 安装 MediaWiki

1. 在网络浏览器中，导航到基本 MediaWiki 文件夹中的“index.php”； 您可以使用 Web 域名（替换下面示例中的“wiki.idc.moe”）或 腾讯云提供的IP ，如下所示：

   ```shell
   sudo yum install php-intl
   sudo yum -y install memcached
   sudo systemctl start memcached
   systemctl enable memcached
   systemctl status memcached
   
   http://124.223.29.238/wiki/index.php
   ```

   {{< note >}}
   如果直接使用腾讯轻量的 IP 来安装 MediaWiki ，但以后想使用域名，您可以通过将 IP 地址更改为下面描述的“LocalSettings.php”文件中的适当域名来实现。
   {{< /note >}}

2. 选择设置链接，然后继续执行设置步骤。 当提示输入数据库服务器时选择 MariaDB 选项，然后输入您为 MediaWiki 创建的数据库名称、用户名和用户密码。

3. 在设置过程结束时出现提示时下载 `LocalSettings.php` 文件，然后将其移动或复制其内容到腾讯云轻量服务器上的 `/var/www/html/wiki/LocalSettings.php`。

4. 设置权限为 664

   ```
   sudo chmod 664 /var/www/html/wiki/LocalSettings.php
   ```

5. 在浏览器中再次访问 `index.php` 以确认 MediaWiki 已成功安装。

## 参考资料

* 用腾讯轻量云搭建 MediaWiki 百科程序 - 主机萌站：[https://blog.idc.moe/archives/build-mediawiki-on-server.html](https://blog.idc.moe/archives/build-mediawiki-on-server.html)
* Download - MediaWiki：[https://www.mediawiki.org/wiki/Download](https://www.mediawiki.org/wiki/Download)
* Manual:Extensions - MediaWiki：[https://www.mediawiki.org/wiki/Manual:Extensions](https://www.mediawiki.org/wiki/Manual:Extensions)
* 扩展手册 - MediaWiki：[https://www.mediawiki.org/wiki/Manual:Extensions/zh?tableofcontents=1](https://www.mediawiki.org/wiki/Manual:Extensions/zh?tableofcontents=1)
* 分类:按分类排列的扩展 - MediaWiki：[https://www.mediawiki.org/wiki/Category:Extensions_by_category/zh](https://www.mediawiki.org/wiki/Category:Extensions_by_category/zh)
* 手册:备份一个维基 - MediaWiki：[https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki/zh?tableofcontents=1](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki/zh?tableofcontents=1)
* mediawiki的备份、升级与恢复：[https://blog.csdn.net/joei4cm/article/details/83982605](https://blog.csdn.net/joei4cm/article/details/83982605)

参考资料：[https://blog.idc.moe/archives/build-mediawiki-on-server.html](https://blog.idc.moe/archives/build-mediawiki-on-server.html)

