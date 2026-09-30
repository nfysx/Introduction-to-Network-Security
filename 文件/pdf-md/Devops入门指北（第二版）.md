# Devops入门指北（第二版）

### 作者：

Reverier Xu

linlinzzo

## 欢迎来到运维的世界！

运维的全称是运行和维护，主要工作是保障各类设备，系统，网络正常运行和可用。运维工程师的重要程度不亚于开发，是真正活跃在软件服务一线的工人。

当我动笔写这么一篇入门指北的时候，我其实不太知道到底应该写些什么。运维的知识太过零碎和繁杂，而且不易系统学习。虽然我在计算机相关专业读了五年书，但是运维技能大多来自日常的瞎倒腾和各种事故处理经验，我到底有没有资格来写一篇运维入门指北，可能还有点存疑。不过来都来了，我尽力给大伙整点感兴趣的东西。

运维所需要具备的技能十分广泛，你可能还需要懂点开发、懂点架构之类的知识。软件工程师更倾向于如何高效率的服务好一个用户，利用算法和架构设计来提供完善的功能，而运维工程师则要负责将这个软件从单一用户服务推广到成千上万的用户，让所有用户可以在同一个服务端点享受相同的服务，互相之间隔离并不受干扰；同时，运维工程师还要负责让服务尽可能的永远运行下去，并且具有良好的错误恢复和负载均衡机制。

作为一名运维，首先你需要学会如何操作自己手上最为灵活和强大的工具 —— 操作系统。接下来，我会介绍一些有关操作系统的使用和常识，并给大家带来一个有趣的运维场景。

## **GNU/Linux**？

由于 GNU/Linux 在服务器运维市场占据主要优势（以及 Windows Server 需要付费使用），因此本指北、本次比赛的所有运维题目均不包含 Windows 运维技巧。

当本指北聊到操作系统时，一般指 GNU/Linux、Windows、Unix（MacOS X），安卓（套皮/原生）等移动端操作系统暂时不做讨论。移动端操作系统为了用户体验牺牲了维持服务稳定性的机制（杀后台），因此暂时不会出现在运维的世界里。

GNU/Linux 是一个开源的操作系统，由社区驱动和维护，并作为基础设施存在于世界的每个角落里。在开始之前，你需要准备一个 Linux 操作系统作为练手的环境。一般而言，推荐使用 [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install) 作为起点，配置简单并且没有损坏你的日常操作系统的风险。另一个可行的方式使用虚拟机安装完整的 Linux 发行版，目前主流的虚拟机软件有 [VirtualBox](https://www.virtualbox.org/)、[VMWare](https://www.vmware.com/) 与 Windows 自带的 [Hyper-V 虚拟机](https://learn.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/about/)。VMWare 虽然已经个人版免费，但是想要下载的话入口藏的比较深；Hyper-V 虚拟机的使用起来相对不爽；这里推荐使用 [VirtualBox](https://www.virtualbox.org/) 作为最初的选择，当你的知识储备足够支撑你认清计算机的世界之后，再换你认为最合适的方案也不迟。

如果你配置了 WSL，可以在 Windows 应用商店中安装 [Debian](https://apps.microsoft.com/detail/9msvkqc78pk6?rtc=1&hl=zh-cn&gl=CN)、Fedora（仅有非官方打包，保险起见这里不贴链接）或者 [Ubuntu](https://apps.microsoft.com/detail/9pdxgncfsczv?hl=zh-cn&gl=CN) 作为发行版前端；如果是虚拟机起步，在安装好虚拟机之后，可以在[这里选](https://mirrors.tuna.tsinghua.edu.cn/#)择各个发行版的操作系统镜像。

### 什么是发行版？

发行版是 Linux 社区根据不同的需求、人群与场景定制出的 GNU/Linux 成品操作系统。你可能会在不同的博客里看到 ~~CentOS（~~ 已死），Ubuntu，Arch Linux，[Debian](https://mirrors.tuna.tsinghua.edu.cn/debian-cd/current/amd64/iso-dvd/)，Fedora 等等不明觉厉的词汇，这些都是不同的 Linux 发行版，他们之间的大型区别仅在于软件包的版本、安装方式和维护团队不同，其他的大同小异，在初学之时不用过于区分。

在你整明白什么是应用程序二进制接口（ABI），以及这个东西会如何影响你之前，本指北仅推荐使用 Debian 作为你最初了解 Linux 系统的发行版。单一版本的 Debian 提供了极其稳定的 ABI 保证与相对较长的生命周期，能够为你避开很多不必要的麻烦；缺点可能是软件包相对老一点（有时候也不是坏事），不过安全更新很及时。

当前（2026年6月27日） Debian 的最新稳定发行版是 Debian 13.5，请使用最新版本。Debian 13 的维护周期一直持续到 2030年6月30日，足够你成长为一代高手。

## 操作系统驾驶员

GNU/Linux 支持图形化的桌面环境，但大家更常用的是终端。无论是 SSH 服务还是桌面上的虚拟终端pty，还是直接上手tty，你不可避免的要接触到命令行界面，打开终端看到的 `$ _` 将是你日后的亲密伙伴。

作为一名熟练的操作系统驾驶员，学会挂挡踩油门加速漂移 ~~翻车跑路是~~ 必不可少的。接下来，你需要先了解 GNU/Linux 发行版独特的包管理器（Debian 的包管理器是具有超级牛力的 APT），然后学习更换软件源、安装常用软件包。在这些完成之后，你就需要尝试摆脱鼠标，双手放在键盘上 ~~当钢琴家了。~~

你需要学习基础文件操作、网络栈等等基础理论，理解 Unix-like 系统中一切皆文件的思想，以及 Linux 下可以通过文件完成的各种神奇技巧；最终成为一名合格的操作系统驾驶员。

下面我们用一个例子来熟悉一下 Linux 基础的命令吧。

## 搭建一个博客？

博客，又称部落格、网络日志，是相对于微博这种社交媒体来说非常古早的东西。在本文档当中，我推荐你建立一个独立博客，而不是在稀土掘金、CSDN、博客园上建一个空间。博客与个人空间不同，个人空间更像是一份简历，而博客是你可以发文章的地方，是你的思想苗圃。

有的博客分享生活，有的博客探讨技术。不论是生活还是技术，搭建一个博客这件事本身对一个运维的成长颇有裨益。如果想有点挑战，你可以尝试在不使用服务器面板（[宝塔](https://www.bt.cn/new/index.html)、[1Panel](https://1panel.cn/) 等）的情况下部署一个博客程序，并让你的朋友在公网访问它。如果还想再难一点，你可以抛弃现有的博客程序（[WordPress](https://cn.wordpress.org/)、[Typecho](https://typecho.org/) 等），自己开发一个。你也不一定要在租赁的云服务器上搭建博客，如果选择 [Hexo](https://hexo.io/zh-cn/)、[Astro](https://astro.build/) 这类静态博客框架的话，你甚至不需要自己购买服务器和域名……搭建完博客之后，这仅仅只是拥有服务器的你的故事的开始。你可以用服务器做很多事情，用命令行驾驶着服务器 ~~旅行。~~

一些主流的服务器面板非常先进，只要安装好面板，点几下等一会一个博客就装好了，感兴趣的同学可以自行搜集相关资料尝试使用服务器面板快速搭建。在这里，我们不妨用我们电脑上的虚拟服务器搭建一个 Typecho 博客吧。这里的命令都经过两次验证，如果不出意外，你照着这里的命令设置就可以成功。Typecho 和 WordPress 都是用 [PHP](https://www.php.net/) 编写的，你可以用几乎相同的方法部署 WordPress。

第一步，我们需要在微软应用商店下载 Debian，安装好后打开 Debian，按照提示设置好一些信息即可。

设置好 Debian 之后，我们需要对 apt 软件源进行更新，这相当于对应用商店的目录进行更新。在此之前，我们可以将 apt 官方源替换成镜像源，后续安装软件会快一点。写入的内容可以在 [Debian 软件源 - 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)查看。在这里，我们用到 `nano` 命令，这是一个文件编辑器，对我这种不会用 [Vim](https://www.runoob.com/linux/linux-vim.html) 的人来说非常友好直白。执行完 `sudo apt update` 之后 Debian 可能会提示一些软件有更新，可以执行 `sudo apt upgrade` 来更新这些软件。

```
moeops@foo:~$ sudo nano /etc/apt/sources.list # 编辑软件源 Ctrl+O 保存 Ctrl+X 退出
```

`sudo` 是什么？

在服务器中，我们是普通用户 moeops，不是超级用户（root）。 `sudo` 命令让我们可以临时使用超级用户的名义做事。[生产环境](https://zhuanlan.zhihu.com/p/165244786)的服务器一般要关闭 root 用户直接登录，遇到需要超级用户的权限才能执行的命令，通常只能使用 `sudo`。

搭建 Typecho，主流的架构方案是 LNMP（Linux + Nginx +MySQL + PHP），L（Linux）的部分我们已经弄好了，接下来我们准备一下其他部分。

```
moeops@foo:~$ sudo apt install php-fpm php-mysql php-mbstring -y # 安装PHP及后续所
需的拓展
moeops@foo:~$ sudo systemctl start php8.4-fpm # 立即启动软件
moeops@foo:~$ sudo systemctl enable php8.4-fpm # 设置开机自启动
moeops@foo:~$ sudo systemctl status php8.4-fpm # 找到软件的service文件地址
moeops@foo:~$ cat /usr/lib/systemd/system/php8.4-fpm.service # 查看service文件
```

上面我们用到了 `cat` 命令，这个命令通常用来查看文本文件的内容。欸， `php8.4-fpm.service` 是文本文件哦。 Linux 与 Windows 系统不一样，不靠文件拓展名来识别文件的类型，遇到一个没有拓展名的文件也是很正常的，例如后面我们新增的 Nginx 配置文件。通过上面的命令，你能够找到类似 `/run/php/php-fpm.sock` 这样的“文件地址（” 严格来说不算一个普通的文件），用个记事本之类的东西记住它，等会要用到。

为什么这里没有版本号？

若你查阅其他的教程，会发现里面的 sock 地址都是带版本号的。我们不妨做一个小实验，执行 `cd /run/php && ls` 到这个目录下一探究竟。

```
moeops@foo:~$ cd /run/php && ls
php8.4-fpm.pid  php8.4-fpm.sock  php-fpm.sock
```

你会发现该目录下同时有带版本号的地址和不带版本号的地址，带不带版本号在这种情况下都是正确的。在 PHP 安装的时候，有一个名为 [php-fpm-socket-helper](https://github.com/deepin-community/php-defaults/blob/master/php-fpm-socket-helper) 的工具会将 `php-fpm.sock` 软链接至 `php8.4-fpm.sock`，避免了更新 PHP 时需要修改 Nginx 配置文件的麻烦。

接下来我们来配置数据库。数据库就像是数据的仓库，我们后续在博客里发的文章都会存在数据库里，我们需要搭建起一个数据库软件并创建一个数据库。在这里，我们设置了数据库名、用户名和密码，后续填写配置信息时需要用到。

```
moeops@foo:~$ sudo apt install mariadb-server -y
moeops@foo:~$ sudo systemctl start mariadb # 将mariadb改为mysql也行
moeops@foo:~$ sudo systemctl enable mariadb
```

```
moeops@foo:~$ sudo mariadb # 同样改为mysql也行

# 进入MariaDB交互命令行后，依次执行以下SQL语句，注意以分号结尾
MariaDB [(none)]> CREATE USER 'moectf'@'localhost' IDENTIFIED BY 'xdsec'; # 创建一
个用户，设置用户名和密码
MariaDB [(none)]> CREATE DATABASE blog; # 创建一个名为blog的数据库
MariaDB [(none)]> GRANT ALL PRIVILEGES ON blog.* TO 'moectf'@'localhost';
MariaDB [(none)]> FLUSH PRIVILEGES;
```

接着，我们可以把博客程序的源代码拉下来。在这里，我们从官方的代码仓库拉取了程序的源代码，并对压缩包使用 `unzip` 解压缩， `ls` 命令用来查看当前目录下的文件， `rm` 命令将没用的压缩包删除。linlinzzo 第一次到这一步设置目录权限的时候不小心执行了 `sudo chmod 750 /usr` （请千万不要执行这行命令），破坏了普通用户对系统公共二进制目录的读取与执行权限，造成了悲剧。运维需要慎重对待每一条执行的命令。

```
moeops@foo:~$ sudo apt install wget unzip -y
moeops@foo:~$ sudomkdir /var/www/html && cd /var/www/html
moeops@foo:/var/www/html$ sudo wget
https://github.com/typecho/typecho/releases/latest/download/typecho.zip
moeops@foo:/var/www/html$ sudo unzip typecho.zip
moeops@foo:/var/www/html$ sudorm typecho.zip
moeops@foo:/var/www/html$ ls
admin  index.php  install  install.php  LICENSE.txt  usr  var
moeops@foo:/var/www/html$ sudochown -R www-data:www-data /var/www/html # 变更文件
夹权限
```

### 最后我们整一整 Nginx。

```
moeops@foo:/var/www/html$ sudo apt install nginx -y
moeops@foo:/var/www/html$ sudo systemctl start nginx
moeops@foo:/var/www/html$ sudo systemctl enable nginx
moeops@foo:/var/www/html$ sudo nano /etc/nginx/sites-enabled/blog # 输入配置文件的
内容
moeops@foo:/var/www/html$ sudo nginx -t # 提示OK了再进行下一步重启
moeops@foo:/var/www/html$ sudo systemctl restart nginx
```

### 配置文件：

```
server {
  listen 80;
  server_name blog.moectf;
  root /var/www/html;
  index index.php;
  location / {
    try_files $uri $uri/ /index.php$is_args$args;
  }
```

```
  location ~ \.php$ {
    include fastcgi.conf;
    fastcgi_pass unix:/run/php/php-fpm.sock; # 填入前面找到的地址
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  }
}
```

这个配置文件是啥意思嘞？我们让 Nginx 监听 80 端口（HTTP 的默认端口），并将 `blog.moectf` 的请求捕获，交给 `/run/php/php-fpm.sock` 处理。

好了，这样 Nginx 就弄好了。由于 `blog.moectf` 不可能通过注册域名得到，所以我们需要修改一下 hosts 文件。在 Debian 运行 `ip addr` 找到 ip 地址，然后在 hosts 文件中加上类似 `172.24.143.236 blog.moectf`。

### 什么是域名？

很久很久以前，人们上网的时候在浏览器输入的还是 IP 地址，类似于 `8.8.8.8` 这样的数字串。人们需要记住一些常用的 IP 地址来上网，非常的麻烦、费脑子。那么有没有什么办法方便人们记住呢？有的兄弟，有的。只要把 IP 地址变成像 `xidian.edu.cn` 这样的域名就好了。怎么变呢？只靠域名是没办法通讯的，需要将域名转换为 IP 地址。我们只要搭建一台机器，这台机器输入为域名，输出为 IP。我们人类只要记住域名就可以了。你可以在域名注册商注册一个域名，这更像是租赁，而不是购买，因为域名有一个租期（最长不超过 10 年）。购买完域名之后，你可以在 DNS 服务器上设置域名的解析记录，将域名的某个子域名直接或间接解析到特定的 IP 地址。

关于域名递归解析过程等具体内容，你可以在网络上搜索相关文章，或者问问 AI。建议阅读 [DNS 原理入门](https://www.ruanyifeng.com/blog/2016/06/dns.html)和 [DNS 查询原理详解](https://www.ruanyifeng.com/blog/2022/08/dns-query.html)。

接下来在浏览器中访问 `http://blog.moectf/` （直接输入 `blog.moectf` 浏览器会认为其是搜索关键词）， Typecho 会将你送到安装页面，安装程序的第一步会检查环境配置，按照刚才的安装步骤这一步应该不会报错。第二步填写数据库配置时，数据库前缀和数据库地址保持默认，用户名为 `moectf`，密码为 `xdsec`，数据库名为 `blog`。随后自行设置管理员账号信息。至此，你已经可以拥有属于你自己的博客了。

但是这时候你向你的舍友分享你的博客，他在他的浏览器上打不开。如何让你的博客在公网上可访问呢？建议阅读[小白搭建个人独立博客的方法](https://feng.pub/0720222845.html)，租赁一个带公网 IP 地址的服务器和域名来实现，也可以通过内网穿透工具来实现。在折腾的过程中，你可以尝试了解 [HTTP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Guides/Overview) 与 [HTTPS](https://www.runoob.com/w3cnote/https-ssl-intro.html)、CDN（Content Delivery Network）这些东西。如果遇到问题了，可以先尝试自己用搜索引擎找找问答或文档，有时 StackOverflow 某个回答的一条评论比先进 AI 模型用在线搜索得到的答案还要切中肯綮。

## 运维题？

我们想设计一些简单的题目让你初步了解和熟悉 Linux 系统，并使用自己擅长的编程语言来解决一些问题。与其他方向的题目不同，这里的题目与除Web以外的安全方向的关系并不是很大。运维题需要做些什么呢？

- 写点代码并在服务器上部署它
- 在 Linux 系统中玩耍

解决一些常见的服务崩溃
