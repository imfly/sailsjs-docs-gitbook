# [Node.js](https://soundcloud.com/marak/marak-the-node-js-rap) 新手?
好吧，让我们帮你“领上道”吧。

官网 [nodejs.org](http://nodejs.org) 的介绍是这样的：
> "Node.js是一个构建在`谷歌javascript运行器`(Chrome's JavaScript runtime)上的平台， 易于构建快速、可扩展的网络应用。Node.js 使用事件驱动，非柱塞 I/O 模型，使得数据密集型实时应用运行在跨分布式设备上（更加）轻量、高效和完美。"

更简单地说，Node.js允许我们在浏览器之外迅速有效地运行JavaScript代码，使得应用同一种语言开发前端和后端成为可能。

## 需要什么操作系统？

Node.js 可以安装在大部分主流的操作系统上，MacOSX，许多流行的 Linux，以及 Windows 都支持。

现在，你可以根据自己的操作系统，选择浏览下面的介绍文章：

 [Mac OSX](http://sailsjs.org/get-started#?install-on-osx)

[Linux](http://sailsjs.org/get-started#?install-on-linux)

 [Windows](http://sailsjs.org/get-started#?install-on-windows)

<h2>
<a id="install-on-osx" name="/getStarted?q=--install-on-osx-" class="anchor" href="http://sailsjs.org/getStarted?q=--install-on-osx-"><span class="mini-icon mini-icon-link"></span></a>
OSX安装
</h2>

使用 [一个包](http://nodejs.org/download/):

_简单 [下载 Macintosh Installer](http://nodejs.org/download/)._

使用 [homebrew](https://github.com/mxcl/homebrew):

    brew install node

使用 [macports](http://www.macports.org/):

    port install nodejs

<h2>
<a id="install-on-linux" name="/getStarted?q=--install-on-linux-" class="anchor" href="http://sailsjs.org/getStarted?--install-on-linux-"><span class="mini-icon mini-icon-link"></span></a>
Linux安装
</h2>

### Ubuntu, Mint

例如:

    sudo apt-get install python-software-properties python g++ make
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

这会将当前稳定的 node.js 安装在当前稳定版的Ubuntu.上。Quantal (12.10) 用户可能需要为`add-apt-repository`命令安装 *software-properties-common* 包，才能运行 `sudo apt-get install software-properties-common`

与包(Amateur Packet Radio Node Program)有一个名字冲突，二进制的nodejs已经将名字从`node`改为`nodejs`。你需要符号链接`/usr/bin/node` 到`/usr/bin/nodejs`，或者卸载 Amateur Packet Radio Node 程序，避免冲突。

### Fedora

Fedora 18 和更新版本，[Node.js](https://apps.fedoraproject.org/packages/nodejs) and [npm](https://apps.fedoraproject.org/packages/npm) 可用。仅仅使用您喜欢的图形包管理器，或在命令行安装即可:

    sudo yum install npm

### RHEL/CentOS/Scientific Linux 6

Node.js and npm 可用于 [Fedora Extra Packages for Enterprise Linux (EPEL)](https://fedoraproject.org/wiki/EPEL) _测试_ 库。如果你还没有这么做，首先[启用EPEL](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F)，然后运行下面的命令：

    su -c 'yum --enablerepo=epel-testing install npm'

### Arch Linux
Node.js 可用在社区库.

    pacman -S nodejs

### Gentoo
Node.js 可用在官方 gentoo 仓库树里，你需要unmask它。

    # emerge -aqv --autounmask-write nodejs
    # etc-update
    # emerge -aqv nodejs

### Debian, LMDE

对于 *Debian sid (不稳定版)*, [Node.js 可用在官方库](http://packages.debian.org/search?searchon=names&keywords=nodejs).

对于 *Debian Wheezy (最新稳定版)*, [Node.js 可用于 wheezy-backports](http://packages.debian.org/wheezy-backports/nodejs). 为了安装 [backports](http://backports.debian.org/Instructions/)，添加下面一行到 sources.list (`/etc/apt/sources.list`):

    deb http://YOURMIRROR.debian.org/debian wheezy-backports main

然后，运行：

    apt-get update
    apt-get install nodejs

对于 *Debian Squeeze (旧稳定版)*，最好自己编译 (as `root`):

    apt-get install python g++ make
    mkdir ~/nodejs && cd $_
    wget -N http://nodejs.org/dist/node-latest.tar.gz
    tar xzvf node-latest.tar.gz && cd `ls -rd node-v*`
    ./configure
    make install

### openSUSE & SLE
[Node.js 稳定版仓库列表](https://build.opensuse.org/package/show?package=nodejs&project=devel%3Alanguages%3Anodejs)。node.js 也可在 openSUSE:Factory repository 中找到。

可用的 RPM 包: openSUSE 11.4, 12.1, Factory and Tumbleweed; SLE 11 (with SP1 and SP2 variations).

例如， 安装在openSUSE 12.1上:

    sudo zypper ar http://download.opensuse.org/repositories/devel:/languages:/nodejs/openSUSE_12.1/ NodeJSBuildService
    sudo zypper in nodejs nodejs-devel

### FreeBSD and OpenBSD
Node.js 可通过ports系统使用。

    /usr/ports/www/node

开发版本也可使用ports

    cd /usr/ports/www/node-devel/ && make install clean

或者FreeBSD上的包

    pkg_add -r node-devel

在FreeBSD上，Node包管理并不默认与 Node.js 一起安装，但对于开发和安装以来还是需要的。

    /usr/ports/www/npm

Also note that FreeBSD 10 using clang will conflict with the occasional build scrpt (which assumes gcc) using node-gyp, and can be resolved by setting an envvar.
还要注意，FreeBSD 10与偶尔使用的构建脚本（好像是gcc，用于node-gyp）冲突，可以通过设置一个环境变量解决。

    CXX=c++

<h2>
<a id="install-on-windows" name="/getStarted?q=--install-on-windows-" class="anchor" href="http://sailsjs.org/getStarted?q=--install-on-windows-"><span class="mini-icon mini-icon-link"></span></a>
Windows安装
</h2>

使用 [一个包](http://nodejs.org/download/):

_简单 [下载 Windows 安装器](http://nodejs.org/download/)._

使用 [chocolatey](http://chocolatey.org) 安装 [Node](http://chocolatey.org/packages/nodejs):

    cinst nodejs

或者 [与NPM一起完全安装](http://chocolatey.org/packages/nodejs.install):

    cinst nodejs.install

## 关于 Sails.js
一旦 Node.js 安装完毕， 就可以继续 [安装 Sails](http://sailsjs.org/get-started#?getting-started-installation)。
 
## 更多帮助
计划赶不上变化，如果你仍然有问题，请. If you still have any issue with this,  请随时访问 node.js  [IRC频道](irc://irc.freenode.net/node.js) 或者 我们的 [IRC 频道](irc://irc.freenode.net/sailsjs).


<docmeta name="displayName" value="New To Node?">
