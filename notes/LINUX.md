# LINUX

### Linux 目录结构

### 文件权限

##### 使用者与群组

文件拥有者(user)

群组概念(group)

其他人的概念(others)

root 万能的天神，不受权限的控制

Linux系统当中，默认的情况下，所有的系统上的账号与一般身份使用者，还有 root 的相关信息， 都是记录在 /etc/passwd 这个文件内的；至于个人的密码则是记录在 /etc/shadow 这个文件下； 此外，Linux 所有的组名都纪录在 /etc/group 内

##### 目录详情字段解释

```shell
drwxrw-r-- 1 root root 1986 May 4 18:00 test.conf
```

1. drwxrw-r--：代表文件类型与权限

   > [d] 表示目录；[-] 表示文件；[l] 表示链接；
   >
   > [b] 表示为装置文件里面的可供储存的接口设备；
   >
   > [c] 表示为装置文件里面的串行端口设备，例如键盘、鼠标；
   >
   > rwxrw-r-- 三个为一组，第一组为文件拥有者可具备的权限，第二组为加入此群组的账号的权限，第三组为非本人且没有加入本群组的其他账号的权限；[r] 代表可读(read)，[w] 表示可写(write)，[x] 表示可执行(execute)，[-] 无权限

2. 表示有多少档名连结到此节点 (i-node)

   > 每个文件都会将他的权限与属性记录到文件系统的 i-node 中，不过，我们使用的目录树却是使用文件名来记录， 因此每个档名就会连结到一个 i-node

3. 表示文件或目录的拥有者账号

4. 表示文件的所属群组

5. 表示文件的容量大小，默认单位为 bytes

6. 表示文件创建或最近修改日期

7. 文件名

##### 改变文件属性与权限

改变文件所属群组 chgrp(change group)

```shell
chgrp users test.txt	#改变 test.txt 文件所属群组为 users，要改变的组名必须在 etc/group 文件内存在
chgrp -R users test.txt	#递归变更，将 test.txt 下所有文件群组改变为 users
```

改变文件拥有者 chown(change owner)

在复制文件时会复制执行者的属性与权限，此时通常需要改变文件拥有者

```shell
chown [-R] users test.txt	#改变 test.txt 文件的拥有者为 users，用户必须存在
chown root:root test.txt	#改变 test.txt 文件的拥有者和群组为 root
```

改变文件权限 chmod(change mode)

```shell
chmod [-R] 777 test.txt	#更改 test.txt 权限为 rwxrwxrwx；r-4,w-2,x-1
chmod u=rwx,go=r test.txt	#更改权限为 rwxr--r--;u-user,g-group,o-others,a-all
chmod a+w test.txt	#所有人添加写入权限
chmod a-x test.txt	#所有人减去执行权限
```

##### 权限对文件和目录的意义

文件权限

r 代表文件内容可读；w 代表可编辑/新增/修改文件内容，但没有删除文件的权限；x 代表文件可执行，同 window 下的 .exe/.bat 等文件

目录权限

r 代表具有读取目录结构列表的权限，即可以查询该目录下的文件名数据；w 代表可更改该目录结构，如创建新文件或目录，删除已有文件目录，重命名已有文件目录，移动文件目录位置；x 代表用户可以进入该目录成为工作目录，工作目录就是目前所在的目录

##### 文件预设权限 umask

文件预设权限：-rw-rw-rw-

目录预设权限：drwxrwxrwx

```shell
umask	#获得数字形态的权限设定分数，它指的是默认值需要减掉的权限
#如 umask 值为0022，则创建的文件默认权限为 -rw-r--r--，创建的目录默认值为 drwxr-xr-x
umask 002	#修改 umask 值为002
umask -S	#以符号形式显示权限
```



### Linux 命令

身份切换

```shell
su -	#切换为 root 身份
exit	#回退到用户身份
```

目录切换命令 cd(change directory)

```shell
cd /usr #进入usr文件夹
cd ../	#返回上层目录
cd /	#返回根目录
cd ~	#返回用户目录
cd -	#返回上一个所在目录
```

显示当前所在目录 pwd(Print Working Directory)

```shell
pwd	#显示当前所在目录
pwd -P	#显示文件真实路径，而非链接路径
```

创建文件夹 mkdir

```shell
mkdir test	#在当前目录下创建一个 test 文件夹
mkdir -m 711 test1	#在当前目录下创建一个权限为711的 test1 文件夹
mkdir -p test1/test2/test3	#在当前目录下递归创建三层文件夹
```

删除目录 rmdir

```shell
rmdir test	#删除空文件夹
rmdir -p test1/test2/test3	#连同上层空目录一起删除
```

执行文件路径变量 PATH

```shell
echo $PATH	#查看 PATH 变量
# 若未在 PATH 中定义指令路径，则必须使用绝对路径或相对路径直接指定执行档档名
PATH = "${PATH}:/root"	#添加 root 目录到 PATH 变量中，这样 root 下的指令可在任何地方执行
# 为什么不在 PATH 中加入 .?工作目录经常切换，不同目录下可能有相同指令，这样执行的指令会有变动
```

查看目录 ls(list)

```shell
ls	#列出当前目录下的文件
ls -a	#列出当前目录下所有文件，包含隐藏文件
ls -l	#列出当前目录下所有文件的详细信息
ls -d	#仅列出目录本身，而不是列出目录内的文件数据
ls -al --full-time /root	#列出root目录下所有文件详细信息，并显示完整时间
```

复制文件或目录 cp(copy)

```shell
#cp [-aipr] source detination
cp -i ~/.bashrc /tmp/bashrc	#复制~下的.bashrc文件到tmp目录下，重命名为bashrc，若文件已存在则发出询问
cp -a /var/log/wtmp .	#复制var/log下的wtmp到当前文件，连同文件的属性和权限，不加选项属性会改变
cp -r /etc /tmp	#持续递归复制，用于目录的复制行为
```

移除文件或目录 rm(remove)

```shell
# rm [-fir] 文件或目录
```

移动或更名文件与目录 mv

```shell
# mv [-fiu] source destination
mv test1 test2	#更改 test1 名称为 test2
mv bashrc test1	#移动 bashrc 文件到 test1 目录
```

文件内容查看

```shell
#cat tac nl more less head tail od
cat -n a.txt	#查看 a.txt 并显示行号
head -n 5 a.txt	#查看 a.txt 前5行
cat -n a.txt | head -n 10 | tail -n 4	#查看 a.txt 前十行的后四行并显示行号
```

修改文件时间或创建文件

```shell
touch a.txt	#创建一个名为 a.txt 的空文件
# touch [-acdmt] 文件
```



```shell

```



