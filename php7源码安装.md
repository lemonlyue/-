# php7源码安装

1. 下载压缩包

`wget  https://www.php.net/distributions/php-7.3.8.tar.bz2`

2. 解压

`tar -xjvf php-7.3.8.tar.bz2`

3. 编译

> ./configure --perfix=/home/study/soft/php
>
> 编译时报了一个错误：
>
> configure: error: libxml2 not found. Please check your libxml2 installation.
>
> 原本以为是缺少libxml2的安装，后来查阅了百度，是缺少了libxml2-dev
>
> 安装命令：
>
> `yum install libxml2-devel `

4. make(构建)

`make`    `make test`  `make install`

5. 查看一下php的扩展

`./bin/php -m`

6. 测试一下

`vi test.php`

```
<?php
echo time();//输出当前的时间
```

+ 结果如下：

```
[root@VM_0_2_centos php]# ./bin/php test.php 
1566919402[root@VM_0_2_centos php]# ./bin/php test.php 
```
7. 简化php命令

+ 编辑目录下的.bash_profile文件

`vi ~/.bash_profile`

```
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH
alias php=/home/study/soft/php/bin/php
```

再执行`source ~/.bash_profile`，简化php命令完成。

8. php7源码安装遇到的坑

+ gcc autoconfig没有安装,或者是其他软件没有安装
+ 上面提到的libxml2-dev的安装
+ 编译后没有php.ini文件

> 返回解压后的php安装包，将php.ini-development复制到安装目录下
>
> `cp php.ini-development /home/study/soft/php/etc/`
>
> 回到安装目录下修改文件名称
>
> ` mv php.ini-development php.ini`
>
> 当前环境的话修改`php.ini`配置不会生效，因为有效的php.ini在安装目录下的`/lib`下
>
> 使用`php -i | grep php.ini`查看
>
> 将php.ini移动到lib下面
>
> `mv ./etc/php.ini ./lib`