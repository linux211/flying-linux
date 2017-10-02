
# 搭建gitbook环境
gitbook是一个很不错的编写上传共享电子书的开源工具，可以部署在unix和windows系统上，部署环境过程主要分两步:

 **- 安装nodejs**

 **- 安装gitbook-cli**

查看系统版本：
```
[root@gitbook books]# cat /proc/version

Linux version 3.10.0-514.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) #1 SMP Tue Nov 22 16:42:41 UTC 2016

```
##  1、安装配置nodejs

我选择免编译的方式，解压就可以直接用。

下载链接：

<https://nodejs.org/en/download/>

选择系统对应的软件版本Linux Binaries (x86/x64) 64-bit
```
[root@gitbook books]#node_version=8.5.0
[root@gitbook books]#tar -xf node-v${node_version}-linux-x64.tar.xz -C /usr/local
[root@gitbook books]#ln -sf /usr/local/node-v${node_version}-linux-x64/bin/node /usr/local/bin/node
[root@gitbook books]#ln -sf /usr/local/node-v${node_version}-linux-x64/bin/npm /usr/local/bin/npm
```
## 2、安装gitbook

### 2.1通过npm安装gitbook
```
[root@gitbook books]#npm install -g gitbook-cli
```
如果由于网络的原因可能会安装失败，这时候更换安装源的话会快些。

更换安装源

### 2.2修改源
```
[root@gitbook books]#cd /usr/local/node-v${node_version}-linux-x64/lib/node_modules/npm/doc/files
[root@gitbook books]#echo 'registry = http://registry.nmp.taobao.org' >>  npmrc.md
```


再次执行安装，当出现下面的结果说明安装成功了。
```
[root@gitbook tmp]# npm install -g gitbook-cli
/usr/local/node-v8.5.0-linux-x64/bin/gitbook -> /usr/local/node-v8.5.0-linux-x64/lib/node_modules/gitbook-cli/bin/gitbook.js
+ gitbook-cli@2.3.2
updated 1 package in 10.511s
[root@gitbook books]#ln -sf /usr/local/node-v${node_version}-linux-x64/bin/gitbook /usr/local/bin/gitbook

```
### 2.3查看gitbook版本
```
[root@gitbook books]# gitbook -V
CLI version: 2.3.2
GitBook version: 3.2.3
```
## 3、gitbook使用

gitbook是使用使用Markdown语法编写的，易于学习

首先创建README.md和SUMMARY.md，前者相当于一本书的简介，而后者相当于一本书的目录

### 3.1编辑目录文件
```
[root@gitbook books]# vim SUMMARY.md
```
```
* [简介](README.md)

* [第一章](chapter1/README.md)

 - [第一节](chapter1/section1.md)

 - [第二节](chapter1/section2.md)

* [第二章](chapter2/README.md)

 - [第一节](chapter2/section1.md)

 - [第二节](chapter2/section2.md)

* [结束](end/README.md)
```
### 3.2初始化gitbook
```
[root@gitbook books]# gitbook  init
info: create README.md
info: create chapter1/README.md
info: create chapter1/section1.md
info: create chapter1/section2.md
info: create chapter2/README.md
info: create chapter2/section1.md
info: create chapter2/section2.md
info: create end/README.md
info: create SUMMARY.md
info: initialization is finished
```
注意要严格按照标准和层次结构编写，例如第一行*和[之间是由分号隔开的，其他详细的语法大家可以参考网上的Markdown语法格式。
```
[root@gitbook books]# tree
.
├── chapter1
│   ├── README.md
│   ├── section1.md
│   └── section2.md
├── chapter2
│   ├── README.md
│   ├── section1.md
│   └── section2.md
├── end
│   └── README.md
├── README.md
└── SUMMARY.md

```
### 3.3 输出为静态网页
```
[root@gitbook books]# gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 8 pages
info: found 0 asset files
info: >> generation finished with success in 0.9s !

Starting server ...
Serving book on http://localhost:4000
```
这时候可以在浏览器中输入http://服务器IP:4000/进行访问了，注意上边的进程通过键盘CTRL+C才可以关闭。
### 3.4修改server监听端口
如果觉得4000端口访问不方便，可使用如下命令修改监控进程端口和开启80端口

```
[root@gitbook books]# gitbook  serve --lrport 4785 --port 80
Live reload server started on port: 4785
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 8 pages
info: found 0 asset files
info: >> generation finished with success in 1.0s !

Starting server ...
Serving book on http://localhost:80
```
想要添加内容的话大家也可以在相应章节的文件添加内容

查看生成的文件，可以看到多了_book目录，这个就是生成的电子书目录，可以迁移该目录到任何你想迁移的地方。
```
[root@gitbook books]# ll
total 8
drwxr-xr-x. 6 root root 107 Sep 24 02:32 _book
drwxr-xr-x. 2 root root  61 Sep 24 02:01 chapter1
drwxr-xr-x. 2 root root  61 Sep 24 01:50 chapter2
drwxr-xr-x. 2 root root  23 Sep 24 01:50 end
-rw-r--r--. 1 root root  16 Sep 23 22:28 README.md
-rw-r--r--. 1 root root 338 Sep 24 01:54 SUMMARY.md
```

进入电子书可以看到有一个index.html文件，该文件就是生成的静态页面
```
[root@gitbook _book]# ll
total 92
drwxr-xr-x.  2 root root    66 Sep 24 02:32 chapter1
drwxr-xr-x.  2 root root    66 Sep 24 02:32 chapter2
drwxr-xr-x.  2 root root    24 Sep 24 02:32 end
drwxr-xr-x. 10 root root   270 Sep 24 02:32 gitbook
-rw-r--r--.  1 root root  9003 Sep 24 02:32 index.html
-rw-r--r--.  1 root root 78435 Sep 24 02:32 search_index.json
```

需要注意的是gitbook serve 消耗大量 CPU 资源，建议直接用gitbook build 然后放到任何支持静态文件的web server上。

### 3.5、使用模拟的web服务器供访问电子书静态页面
```
[root@gitbook books]#gitbook build
```
使用python模拟web服务器
```
[root@gitbook books]#cd _book
[root@gitbook books]#python -m SimpleHTTPServer 4000
```
这样就可以在浏览器中输入http://服务器IP:4000/index.html进行访问电子书的静态页面了

### 3.6、衍生

像上面的目录结构还是相对简单的，如果遇到篇幅比较长，目录结构复杂的电子书的话，例如下面展示的目录结构，在linux系统内编写的话就显得太过繁琐，有没有图形化的工具可以供我们使用编写电子书，这样就简单多了，图形化的gitbook电子书编写工具很多，我下篇文章为大家讲解gitbook editor的使用方法。
```
[root@gitbook books]#vi SUMMARY.md
```
```
# Summary

* [Introduction](README.md)

* [第一章：安装ansible并测试](ch1/install_ansible.md)

    * [1. 查看系统版本](ch1/check_release.md)

    * [2. 安装ansible配置yum配置](ch1/cfg_ansibleyum.md)

    * [3. 安装ansible](ch1/install.md)

    * [4. ansible安装后测试](ch1/test.md)

    * [小结](ch1/WRAPUP.md)

* [第二章：安装tower](ch2/install_tower.md)

    * [1. 资源下载](ch2/download.md)

    * [2. 解压缩tower引导文件](ch2/unzip_guidefile.md)

    * [3. 配置安装tower的yum源](ch2/cfg_toweryum.md)

    * [4. 部署tower](ch2/deploy_tower.md)

    * [1. 资源下载](ch2/download.md)

* [第三章：导入licenses](ch3/import_licenses.md)

* [结束](end/SUMMARY.md)
```

