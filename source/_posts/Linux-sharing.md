---
title: Linux知识分享（转载）
categories:
  - 系统
tags:
  - Linux
  - 教程
cover: /img/banner.png
abbrlink: e9d015c9
date: 2024-01-10 13:20:22
---


## 一、初见Linux目录结构

Linux系统中的目录跟Windows中文件夹的概念差不多。但两者的目录结构有很多不同。

**Windows目录结构**

Windows的目录结构采用的树形结构。在Windows中，我们会将一块磁盘分为若干逻辑盘，赋予每个逻辑盘一个盘符如`C:`，`D:`等。然后每一个逻辑盘中都有一个根目录。

**Linux目录结构**

```
Linux目录结构同样采用的是树形结构。但是Linux系统只有一个根目录，虽然同样有分区的概念，但是与Windows不同，Linux的分区需要通过挂载到某一个目录（也成为挂载点）才能使用。有关目录以及分区的概念在后续的磁盘管理中会详细介绍。
```

**差异性**

- Windows中文件和目录名称不区分大小写。Linux严格区分大小写。
- Windows以 `\`作为分隔符；Linux以 `/`作为分隔符。

**相对路径与绝对路径**

绝对路径：以根开始的路径，在系统中任何位置使用。

```bash
eg:/etc/sysconfig/selinux
```

相对路径：不以根开始的路径，只有在特定的位置才可以使用

```bash
eg:sysconfig/selinux
```

部分特殊符号

- `~`：当前用户的家目录
- `-`：上次所在的目录
- `.`：当前目录
- `..`：上一级目录

#### 常用目录及作用

|目录名|作用|
|:-:|:-:|
|/root|root用户的家目录，存储root的用户文件|
|/home|普通用户的家目录存放位置，该目录会以用户的名称作为子目录；存储普通用户的文件|
|/etc|系统的配置目录|
|/boot|系统引导目录，内核和应道程序存放位置|
|/usr/local|用户通过编译安装存放的路径|
|/usr/bin|普通用户可以执行的命令文件|
|/usr/sbin|管理员可以执行的命令文件|
|/dev|系统设备目录|
|/vat|存放服务的数据和系统日志|

## 二、Linux命令语法

### 基本语法

Linux的所有命令都有如下语法

```bash
command [options] [arguments]
命令		   选项		 参数
```

1.命令、选项和参数

1）命令是用于实现某种功能，选项用于修改命令的默认行为，参数用于指定命令操作的对象，这三者之间使用空格隔开。

2）Linux严格区分大小写

3）可以使用 `;`分隔多条命令，可以在通一行输入多条命令并执行。

2.选项格式

1）短格式选项通常以单破折号 `-`开头，后面紧跟选项字符，多个选项可以组合使用，之间没有空格。

```bash
 eg：
 ls -l -a 等同与 ls -la
```

2）长格式选项使用双破折号 `--`，后跟完整选项名字。

```bash
eg：
ls --all
```

3.必选与可选

1）尖括号 `<>`表示必选项或参数，方括号 `[]`表示可选项或参数。

2）大写的参数表示变量，必选。

4.选项参数

1）短格式选项后跟参数值来设定该选项的参数。

```bash
eg：
查看passwd文件的前5行。
[root@localhost ~]# head -n 5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin

```

2）长格式选项后紧跟=及其参数值来设当该选项的参数。

```bash
eg：
查看passwd文件的前5行。
[root@localhost ~]# head --lines=5 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
```

5.多选项

1）当选项或参数后面出现三个点 `...`表示该选项和参数可以同时有多个。

```bash
eg：
 cat [OPTION]... [FILE]...
 选项和参数都可以有多个
```

2）当选项或参数中出现类似 `a|b|c`的格式，使用 `|`分隔，表示选项或参数可选值只能选择一项。

6.多括号嵌套

在下面date命令的语法中可以发现，在参数中有多个中括号。那么在Linux语法规则中，当出现多个中括号并嵌套时，优先匹配中括号外的内容。

```bash
可以使用date命令来修改系统时间，语法如下：
date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
其中：
MM表示月份，取值01到12
DD表示天,取值01到31
hh表示小时，取值00到23
mm表示分钟，取值00到59
CC表示世纪，取值19~22
YY表示年份，取值00到99
.ss表示秒，取值00到59
eg：
将系统时间修改为12月31日12点30分。
[root@localhost ~]# date 12311230
2023年 12月 31日 星期日 12:30:00 CST

将系统时间修改为2022年12月31日12点30分。
[root@localhost ~]# date 123112302022
2022年 12月 31日 星期六 12:30:00 CST

将系统时间修改为2022年12月31日12点30分30秒。
[root@localhost ~]# date 123112302022.30
2022年 12月 31日 星期六 12:30:30 CST

```

## 三、Linux命令帮助

    不要尝试去记住Linux的所有命令，以rhel9为例系统的内置命令数量在3000到5000个之间，其中最常用的命令有200~300个，并且每个命令都有不同的选项。单靠记忆很难记住所有的命令以及选项，因此我们需要借助一下Linux系统中内置的一些帮助。下面我们依次来介绍这些帮助。

### whatis

作用：用于显示指定命令的描述信息。

语法：

```bash
whatis command
eg:
[root@localhost ~]# whatis cat
cat (1)              - concatenate files and print on the standard output
cat (1p)             - concatenate and print files
```

注意：

- whatis只用于显示命令的简短描述，不会提供使用方式。
- whatis是基于数据库的查询，需要定时去更新数据库。
- 在安装后并不能马上使用需要先手动更新一个数据库。  
    更新数据库命令：`mandb`

### help

作用： 用于显示命令的常用语法，选项和参数

语法：

```bash
command --help
command -h
eg：
[root@localhost ~]# cat --help
用法：cat [选项]... [文件]...
连接所有指定文件并将结果写到标准输出。

如果没有指定文件，或者文件为"-"，则从标准输入读取。

  -A, --show-all           等效于 -vET
  -b, --number-nonblank    对非空输出行编号，同时取消 -n 选项效果
  -e                       等效于 -vE
  -E, --show-ends          在每行结束处显示"$"
  -n, --number             对输出的所有行编号
  -s, --squeeze-blank      不输出多行空行
  -t                       与 -vT 等效
  -T, --show-tabs          将跳格字符显示为^I
  -u                       (被忽略)
  -v, --show-nonprinting   使用^ 和M- 引用，除了LFD和 TAB 之外
      --help		显示此帮助信息并退出
      --version		显示版本信息并退出

示例：
  cat f - g  先输出f 的内容，然后输出标准输入的内容，最后输出g 的内容。
  cat        将标准输入的内容复制到标准输出。

GNU coreutils 在线帮助：<https://www.gnu.org/software/coreutils/>
请向 <http://translationproject.org/team/zh_CN.html> 报告任何翻译错误
完整文档 <https://www.gnu.org/software/coreutils/cat>
或者在本地使用：info '(coreutils) cat invocation'
```

注意：

- 部分命令无法使用 `-h`获得帮助，会提示使用 `--help`。
- `-h`和 `--help`只会提供常用参数和选项，并不是命令的完整帮助。
- 当命令选项和参数是必选时，之间输入命令也可以获得相同的帮助信息

### man

- 作用： 获取系统参考手册，非常详细的Linux命令帮助手册，里面收录了命令的几乎所有参数。  
    man帮助手册存放目录：/usr/share/man  
    语法：

```bash
man command [章节号]
```

man帮助一共分为以下章节。

- man1：用户命令手册页，介绍用户级命令用法，如ls，cd，grep等。
    
- man2：系统调用手册页，介绍内核提供的系统调用规范，用于程序开发。
    
- man3：库调用手册页，介绍各种程序库调用的方法，供程序开发使用。
    
- man4：特殊文件手册页，介绍各种特殊设备文件的规范，如终端，磁盘等。
    
- man5：文件格式手册页，介绍各种配置文件格式，如/etc/passwd等。
    
- man6：游戏手册，介绍各种游戏的用法。
    
- man7：杂项手册页，介绍各种杂项参考信息，如ASCII码表、字符集等。
    
- man8：管理命令手册页，介绍系统管理员使用的命令，如service，sshd等
    
- man9：内核例程手册页，介绍内核特有的例程，供内核开发人员参考。
    
    其中还可以看到某些man手册后带有p或者x，如man1p，man1x。  
    其中以p结尾表示该手册是POSIX命令手册页，介绍复合POSIX标准的命令语法。  
    以x结尾表示扩展命令手册页，介绍扩展命令集的用法。  
    以上所有手册页我们最常用的是man1，man5和man8。
    

每一个man手册包含了以下信息。

|名称|作用|
|:-:|:-:|
|NAME|命令名称以及作用，whatis实则是调用man帮助的NAME字段|
|SYNOPSIS|语法，介绍命令的语法|
|DESCRIPTION|参数以及介绍，介绍参数的作用|
|EXAMPLES|示例，演示命令的用法|
|AUTHOR|作者,该man手册的编写者|
|REPORTING BUGS|报告错误，发现手册错误的提交方式|
|COPYRIGHT|版权说明|
|SEE ALSO|相关文档|

## 四、Linux基本命令

由于Linux系统更多应用在服务器上，我们操作Linux系统通常都是使用远程连接工具，通过ssh等方式登录系统进行使用，当我们通过ssh登录系统的时候，即使安装了桌面也是显示一个纯字符的界面，所以我们需要去学习Linux命令。

### 命令行基本知识

`[root@localhost ~]#`解释如下：

- `@`之前显示的是用户名，这里显示root表示现在是root用户在使用该终端。
- `@`之后是主机名，系统安装完成后，默认主机名为 `localhost`
- 主机名之后为当前所处的目录。`~`表示当前用户的家目录，root用户的家目录在 `/root`。
- 最后的 `#`号表示当前用户身份为超级管理员，普通用户的提示符为 `$`。

#### 目录查看与切换

##### pwd

作用：查看当前所在的目录。  
语法：

```
pwd
eg：
[root@localhost ~]# pwd
/root
```

##### cd

作用：切换目录。  
语法：

```
cd [目录]
eg： 
切换到/opt目录
[root@localhost ~]# cd /opt
[root@localhost opt]#

切换到当前用户的家目录
[root@localhost opt]# cd ~ （cd）
注意：当cd命令不携带任何参数时候，默认回到当前用户的家目录。

切换到上一次所在的目录
[root@localhost ~]# cd -
/opt
[root@localhost opt]# 
```

#### 查看文件和目录的内容

##### ls

作用：查看目录下的文件和目录  
语法：

```
ls [OPTION]... [FILE]...` 
常用选项：

-a      显示目录下的所有文件，包括隐藏文件。
-A      列出除 . 及 .. 以外的任何项目。
-l      显示目录下文件和目录的详细信息。
-d      显示目录的详细信息，而不是目录内的文件。
-i      显示目录下文件以及inode号（索引编号）。
-h      以易于阅读的格式输出文件大小，与-l一起使用。

eg： 
查看当前目录下的文件和目录
[root@localhost ~]# ls
公共  模板  视频  图片  文档  下载  音乐  桌面  anaconda-ks.cfg

查看指定目录下的文件和目录
[root@localhost ~]# ls /
afs  boot  etc   lib    media  opt   root  sbin  sys  usr
bin  dev   home  lib64  mnt    proc  run   srv   tmp  var

查看目录下的所有文件包括隐藏文件
[root@localhost ~]# ls -a
.     视频  音乐             .bash_logout   .config   .ssh          .xauthU8L7m0
..    图片  桌面             .bash_profile  .cshrc    .tcshrc
公共  文档  anaconda-ks.cfg  .bashrc        .lesshst  .viminfo
模板  下载  .bash_history    .cache         .local    .xauthmrFQ44

查看目录除`.`和`..`以外的文件。
[root@localhost ~]# ls -A
公共  图片  音乐             .bash_history  .bashrc  .cshrc    .ssh      .xauthmrFQ44
模板  文档  桌面             .bash_logout   .cache   .lesshst  .tcshrc   .xauthU8L7m0
视频  下载  anaconda-ks.cfg  .bash_profile  .config  .local    .viminfo

查看目录下文件和目录的详细信息
[root@localhost ~]# ls -l
总用量 4
drwxr-xr-x  2 root root    6  6月  8 09:33 公共
drwxr-xr-x  2 root root    6  6月  8 09:33 模板
drwxr-xr-x  2 root root    6  6月  8 09:33 视频
drwxr-xr-x  2 root root    6  6月  8 09:33 图片
drwxr-xr-x  2 root root    6  6月  8 09:33 文档
drwxr-xr-x  2 root root    6  6月  8 09:33 下载
drwxr-xr-x  2 root root    6  6月  8 09:33 音乐
drwxr-xr-x  2 root root    6  6月  8 09:33 桌面
-rw-------. 1 root root 1022  5月 25 19:04 anaconda-ks.cfg
 

显示目录下文件以及inode号
[root@localhost ~]# ls -i
34957596 公共  34957597 视频  51246008 文档      8343 音乐  18096142 anaconda-ks.cfg
18122225 模板  18122230 图片      8342 下载  51246007 桌面

部分通配符符号
- `？`代表任意一个字符，有且一个字符
- `*` 代表数个字符，可以没有也可也是一个或多个
- `[]` 表示可以匹配字符组中的任意一个字符

[root@localhost opt]# ls a
a
[root@localhost opt]# ls a?
ab
[root@localhost opt]# ls a??
abc
[root@localhost opt]# ls a*
a  ab  abc
[root@localhost opt]# ls file[1,5]
file1  file5
[root@localhost opt]# ls file[1,5,3]
file1  file3  file5
```

##### cat

作用：查看指定文件内容

语法：

```
cat [OPTION]... [FILE]...
eg：
[root@rhce ~]# cat /etc/hosts 
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.211.129	rhce
```

#### 创建文件与目录

文件和目录的命令规则：

- 文件和目录的名称字符不能超过255个字符。
- 文件名的名称可以使用除了 `/`以外的任意字符，都是有效的文件名。但是不建议使用使用特殊的字符作为文件名（`$` `*` `&` `%`），如果需要使用，需要使用单引号引起来。
- 文件没有后缀名的称谓，文件名就是一个单纯的名称，除了名称没有任何意义。

##### mkdir

作用：创建目录  
语法：

```
mkdir [OPTION]... DIRECTORY...

常用选项：
-p：递归常见目录
-m：创建的时候指定权限

eg： 
创建一个目录
[root@localhost ~]# mkdir dir1
[root@localhost ~]# ls
公共  模板  视频  图片  文档  下载  音乐  桌面  anaconda-ks.cfg  dir1  passwd  root

递归创建目录
[root@localhost ~]# mkdir a/b/c/d/e
mkdir: 无法创建目录 “a/b/c/d/e”: 没有那个文件或目录
[root@localhost ~]# mkdir -p a/b/c/d/e
[root@localhost ~]# tree a
a
└── b
    └── c
        └── d
            └── e

4 directories, 0 files
```

##### touch

作用：创建文件或更改时间戳  
语法：

```
touch [OPTION]... FILE...
eg： 
创建一个文件
[root@localhost opt]# touch file
[root@localhost opt]# ls
file
```

##### 时间戳

在Linux中所有文件和目录都拥有时间戳，可以使用stat命令查看文件时间戳

- atime（Access time）:文件最后一次查看时间
- mtime（Modify time）:文件内容最后一次修改时间
- ctime（Change time）:文件属性最后一次修改时间
- Birth（Brith time）：文件创建时间（RHEL9开始存在）

```
[root@rhce ~]# stat text
  File: text
  Size: 9         	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 18066441    Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Context: unconfined_u:object_r:admin_home_t:s0
Access: 2023-08-17 10:51:31.710467868 +0800
Modify: 2023-08-17 10:48:14.883740433 +0800
Change: 2023-08-17 10:48:14.883740433 +0800
 Birth: 2023-08-17 10:15:00.033224929 +0800
```

#### 操作文件与目录

##### cp

作用：复制文件或目录  
语法：

```
cp [OPTION]... [-T] SOURCE DEST` 
cp [OPTION]... SOURCE... DIRECTORY` 
cp [OPTION]... -t DIRECTORY SOURCE...` 

常用选项：
-i：如果指定复制的源目录或文件与目标的目录或文件同名，则会先询问是否覆盖旧文件，输入 y 表示直接覆盖，输入 n 表示取消该操作。
-v：显示复制的过程的详细信息
-f：如果指定复制的源目录或文件与目标的目录或文件同名，不询问是否覆盖，直接覆盖
-r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
-p：指定保留的属性
-a：保留全部属性

eg: 
将`etc`目录下`passwd`复制到`opt`目录下
[root@localhost ~]# cp /etc/passwd /opt/
[root@localhost ~]# ls /opt/
passwd

将root目录所有内容复制到opt目录下
[root@localhost ~]# cp -rv /root /opt/
[root@localhost ~]# ls /opt/
passwd  root
```

##### mv

作用：移动文件或目录，修改文件或目录的名称  
语法：

```
mv [OPTION]... [-T] SOURCE DEST` 
mv [OPTION]... SOURCE... DIRECTORY` 
mv [OPTION]... -t DIRECTORY SOURCE...` 

常用选项：
 -i：如果指定移动的源目录或文件与目标的目录或文件同名，则会先询问是否覆盖旧文件，输入 y 表示直接覆盖，输入 n 表示取消该操作。
 -b：当目标文件或目录存在时，在执行覆盖前，会为其创建一个备份。
 -n：如果指定移动的源目录或文件与目标的目录或文件同名，不会询问，直接覆盖旧文件。

eg： 
将`opt`目录下的`passwd`文件移动到`root`目录下
[root@localhost ~]# mv /opt/passwd /root/
[root@localhost ~]# ls /root/passwd 
/root/passwd
```

##### rm

作用：删除文件或目录  
语法：

```
rm [OPTION]... [FILE]...`

常用选项：
-f 强制删除。忽略不存在的文件，不提示确认
-i 每次删除前提示确认
-r 递归删除目录及其内容

eg： 
删除文件
[root@localhost opt]# ls
file
[root@localhost opt]# rm file
rm：是否删除普通空文件 'file'？y
[root@localhost opt]# ls
[root@localhost opt]# 

删除目录
[root@localhost opt]# mkdir dir
[root@localhost opt]# rm -r dir/
rm：是否删除目录 'dir/'？y
```

##### rmdir

作用：删除空目录  
语法：

```
rmdir [OPTION]... DIRECTORY...
常用选项：

-p 递归删除空目录

eg:
[root@localhost opt]# mkdir dir1
[root@localhost opt]# mkdir dir2
[root@localhost opt]# touch dir1/file
[root@localhost opt]# rmdir dir1
rmdir: 删除 'dir1' 失败: 目录非空
[root@localhost opt]# rmdir dir2
[root@localhost opt]# ls
dir1

注：rmdir的意义：rm -r和rmdir都可以删除目录，唯一区别就是rmdir只能删除空目录。使用rmdir删除非空目录是会
提示Directory not empty，但rm -r删除非空目录是没有提示的。使用rmdir删除目录是可以最大话避免误删文件。
```

#### 其他基本命令

##### history

作用：查看历史命令

```
[root@rhce ~]# history 
    1  cat /etc/redhat-release 
    2  lsblk
    3  systemctl enable cockpit
    4  systemctl enable cockpit --now
    5  useradd user1  && echo redhat | passwd --stdin user1
    6  userdel -r user1
    7  id user2
    8  userdel -r user2
    9  cat /etc/passwd
...

```

##### date

作用：查看当前时间

```
[root@rhce ~]# date
Sat  9 Sep 14:39:58 CST 2023
```

##### cal

作用：查看日历

```
[root@rhce ~]# cal
   September 2023   
Mo Tu We Th Fr Sa Su
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30   
```

##### 修改Linux默认语言

```
临时修改
修改成中文
[root@rhce ~]# export LANG="en_GB.UTF-8"
修改成英语
[root@rhce ~]# export LANG="zh_CN.UTF-8"

永久修改
编辑/etc/locale.conf文件，在文件中写入以下内容，保存退出。
LANG="zh_CN.UTF-8"
```

##### 修改Linux时区

```
[root@rhce ~]# timedatectl set-timezone Asia/Shanghai
```

## 五、Linux使用技巧

（1）在Linux中执行一条命令，没有任何回显，大部分情况下该命令执行成立。或者使用 `echo $?`来查看是否执行成功。当回显结果为0是表示执行正确，如果为0表示执行错误。在man帮助中会表明回显表示什么错误。

```
[root@rhce kernel]# cd
[root@rhce ~]# echo $?
0
```

（2）上下方向键，可以通过方向键执行已经执行过的命令。也可以通过历史记录来执行命令。

```
[root@rhce ~]# history 
91  stat text1

！+条目：执行指定的历史命令
[root@rhce ~]# !91
stat text1
  File: text1
  Size: 7         	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 18068553    Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Context: unconfined_u:object_r:admin_home_t:s0
Access: 2023-08-17 10:51:33.051472282 +0800
Modify: 2023-08-17 10:50:51.276910365 +0800
Change: 2023-08-17 10:50:51.276910365 +0800
 Birth: 2023-08-17 10:50:51.276910365 +0800

!da		执行最近一条以da开头的历史记录
[root@rhce ~]# !da
date
Sat  9 Sep 15:27:27 CST 2023
```



