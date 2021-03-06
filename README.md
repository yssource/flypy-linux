# 小鹤音形Linux Rime挂接版镜像

本仓库为小鹤双拼Linux Rime挂接版镜像,挂接内容来自官方网盘。 

- 网盘地址：
http://flypy.ys168.com/  

Linux小鹤音形无直接对应程序，需要挂载Rime使用。

Rime是个优秀的输入工具，在**MacOS**/**Windows**/**GNU/Linux**/**Andrido**/**IOS**平台都有对应的实现。  

Linux平台的Rime依赖Fcitx或IBus。（本仓库挂接版为Fcitx配置，IBUS请自行测试）

> 小鹤双拼挂载官方提供的是基于Fcitx框架



## 更新记录

- 官网更新说明:
https://flypy.com/bbs/forum.php?mod=viewthread&tid=8&extra=page%3D1


## 目录结构

```
rime-data
├── build
│   ├── flypydz.prism.bin
│   ├── flypydz.reverse.bin
│   ├── flypydz.table.bin
│   ├── flypyplus.prism.bin
│   ├── flypyplus.reverse.bin
│   ├── flypyplus.table.bin
│   ├── flypy.prism.bin
│   ├── flypy.reverse.bin
│   └── flypy.table.bin
├── default.yaml
├── flypyplus.schema.yaml
├── flypy.schema.yaml
├── flypy_sys.txt
├── flypy_top.txt
├── flypy_user.txt
└── rime.lua

```

## 测试环境

已经在Archlinux下测试可以正常使用。

- Archlinux
    - Manjaro
    - Arch分支

- 其他GNU/Linux发型版请测。

## Fcitx(4)安装
注意Fcitx分4和5版本，默认Fcitx的版本是Fcitx4（以下Fcitx默认指Fcitx4),安装Fcitx5才是5.但是版本显示都是4.xx。

下面开始的是fcitx4版本的安装过程，如果使用的是fcitx5请直接点击fcitx5安装。

- [fcitx5安装](#fcitx5安装)


- Fcitx输入法框架
- Rime输入法

在**Archlinux**及**Arch分支**(例如**Manjaro**)下安装

```sh
$ sudo pacman -S fcitx-rime
$ sudo pacman -S fcitx-configtool
```

或者使用`yay`作为包管理工具:
```sh
$ sudo yay fcitx-rime
$ sudo yay fcitx-configtool
```

## 配置小鹤双拼挂载

将GNU/Linux版本的小鹤双拼文件下载后，进入**rime-data**文件夹。



将**rime-data**目录下的文件复制到用户目录下的**Rime**配置**rime-data**文件夹

```sh
$ cd rime-data
$ cp -r * ~/.config/fcitx/rime
```
> 提醒:不要将配置文件移动到系统配置目录 ~~/usr/share/rime-data/~~

复制个人配置文件
```sh
$ cp default.yaml default.custome.yaml
```
> 提醒:不要直接编辑**default.yaml**配置文件，不然以后更新时会被覆盖。

重启**Fcitx**,并切换到**Rime**输入法。(右键fcitx托盘重启和选择输入法）。

到了这一步，应该可以使用**Fcitx-Rime**小鹤双拼了。  

但是查看启动日志还是会有问题。

### fcitx-rime日志
日志在**/tmp**下，以**rime.fcitx-rime**开头。

最新的日志对应着3个软链接文件，可以直接查询对应不同的日志级别日志:
```sh
$ cat rime.fcitx-rime.ERROR  # 错误级别
$ cat rime.fcitx-rime.INFO  # 普通级别
$ catrime.fcitx-rime.WARNING # 警告级别
```

直接查看错误级别的日志信息:
```sh
$ tail -f  rime.fcitx-rime.ERROR
Log file created at: 2019/12/18 17:33:01
Running on machine: maojun-pc
Log line format: [IWEF]mmdd hh:mm:ss.uuuuuu threadid file:line] msg
E1218 17:33:01.904620 19037 dict_compiler.cc:46] source file '/home/maojun/.config/fcitx/rime/flypy.dict.yaml' does not exist.
E1218 17:33:01.916185 19037 dict_compiler.cc:46] source file '/home/maojun/.config/fcitx/rime/flypyplus.dict.yaml' does not exist.
E1218 17:33:02.072552 19021 engine.cc:349] error creating translator: 'lua_translator'
E1218 17:33:02.072582 19021 engine.cc:349] error creating translator: 'lua_translator'
```
发现有有码表文件不存在错误和lua脚本加载错误。

码表文件不存在问题是由于rime加载的目录的用户配置主目录，而码表文件在**build**文件中,把**build**文件夹下的所有**bin**文件移到上一层即可。
```sh
$ cd rime-date/build
$ mv *.bin ,,.
```
具体讨论可以参考:
https://flypy.com/bbs/forum.php?mod=redirect&goto=findpost&ptid=516&pid=2354



## Fcitx(4)配置

**美化设置**

Fcitx用户目录位置：

```sh
 ~/.config/fcitx/rime/             
 ```

rime的皮肤用的是fcitx的皮肤，自定义皮肤目录为:~~/usr/share/fcitx/skin~~
```sh
$ ~/.config/fcitx/skin
```

> 提醒:如果没有目录直接创建一个或者从系统皮肤文件夹复制一份~~/usr/share/fcitx/skin~~
> 但不要直接在系统配置文件上修改

**Rime配置**

禁用Rime的英文模式
rime的英文模会将`Shift`键触发为rime的英文模式。
从输入方案中 `engine/processor`列表里注释掉`ascii_composer`




## Fcitx5安装

### 安装Fcitx-rime及rime-lua

Installation process of librim-lua plugin with fcitx 5.xx

For Archlinux/Using pacman as a package management system

1. uninstall fcitx4 and its dependencies.
2. install fcitx5 and its dependencies.

```sh
sudo pacman -S fcitx5-git fcitx5-gtk-git fcitx5-qt5-git fcitx5-rime-git
```
3. Remove only librime
```sh
$ sudo pacman -Rdd librime
```
4. install librime & librime-lua plugins
```sh
$ git clone https://github.com/rime/librime.git
$ cd librime
$ git checkout 1.5.3  # important(Maybe the above problems will be fixed in a future version)
$ ./install-plugins.sh hchunhui/librime-lua
$ make merged-plugins
$sudo make install
```
注意，安装并配置了让Rime支持Lua插件。如果用Fcitx4安装会无法启动报错。用Fcit5安装需要先卸载通过二进制包安装的librime,然后手动从源码编译lua插件并安装。
参考：https://github.com/hchunhui/librime-lua/issues/25#issue-540065284


###  Fcitx5配置
fcitx5的配置在:
```sh
~/.config/fcitx5
```

> 注意:修改配置的时候先关闭Fcitx5,否则重启Fcitx5时配置会被重置。

但是fcitx5的词库目录及皮肤主题目录在:
```sh
~/.local/share/fcitx5
```






