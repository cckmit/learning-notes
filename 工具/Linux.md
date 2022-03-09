# 目录功能

- 绝对路径：从“/”根目录开始逐层查找文件和目录。 

- 相对路径：以当前目录或上一级目录为基准逐层查找文件和目录 当前目录：“./” 当前目录的上一级目录：“../”。

| 目录名称    | 功能与作用描述                                               |
| ----------- | ------------------------------------------------------------ |
| /bin        | 二进制应用程序目录，其中包含二进制文件，**所有用户使用的命令都在这个目录下**。 |
| /boot       | **开机启动**引导目录，启动(boot)配置文件，其中包括了Linux内核文件与开机所需要的文件。 |
| /dev        | **设备目录**，设备(device)相关的文件和目录，其中包含了所有应用程序的配置文件，还包含了启动/停止某个程序的shell脚本。 |
| /etc        | **配置文件**目录，配置文件、启动脚本等文件。                 |
| /home       | **本地用户目录**，所有不同权限的系统用户可在home目录分配存储个人用户的文件和资料。 |
| /lib        | 系统使用的函数库的目录，程序在执行过程中，需要调用一些额外的参数时需要函数库的协助。 |
| /lost+fount | 系统异常产生错误时，会将一些遗失的片段放置于此目录下，通常这个目录会自动出现在装置目录下。如加载硬盘于/disk中，此目录下就会自动产生目录/disk/lost+found。 |
| /media      | 挂载可移动介质(media)，诸如CD、数码相机，软盘，光盘等，可移除设备挂载目录。 |
| /mnt        | **挂载(mounted)文件系统**，临时安装目录，系统的管理员可拥有挂载文件系统的权限。 |
| /opt        | **第三方软件安装目录，第三方应用程序一般放在此目录下，但实际中通常习惯放在/usr/local目录下。** |
| /proc       | 特殊的动态信息目录，此目录的数据都在内存中，如系统核心，外部设备，网络状态，用以维护系统信息和状态，包括当前运行中进程(processes)信息。 |
| /root       | **root用户主文件夹**，读作“slash-root”，其他用户均放置在/home目录下 |
| /run        | 系统运行的时候所需的文件，以前在/var/run中，后来拆分成独立的/run目录，重启后重新生成对应的目录数据。 |
| /sbin       | 重要的系统二进制(systembinaries)文件，也是包含的二进制可执行文件。在这个目录下的linux命令通常都是由系统管理员使用的，对系统进行维护。 |
| /srv        | 服务启动后需要访问的数据目录。                               |
| /sys        | 系统(system)文件，跟proc一样虚拟文件系统，记录核心系统硬件信息。 |
| /tmp        | 临时文件目录，存放临时文件目录，所有用户对该目录均可读写。   |
| /usr        | **应用程序放置目录**，包含绝大部分所有用户(users)都能访问的应用程序和文件。 |
| /var        | 经常变化的(variable)文件，存放系统执行过程经常改变的文件，代表变量文件。在这个目录下可以找到内容可能增长的文件。 |

# 基础命令

- 关闭系统（root 权限）：halt；

- 重启系统（root 权限）：reboot；

- 立即关机：poweroff ；

- 查看用户 id：id -u，root 用户的 id 是 0；

- 查看当前 Linux 主机名：hostname；

- 查看当前用户名：whoami；

- 查看当前日期和时间：date；

- 查看当前目录下所有文件：ls；

  - 显示隐藏文件：ls -a；

  - 显示文件详细信息：ls -l；

  - 显示文件大小：ls -h；

  - 显示文件最近修改时间：ls -t；
  
- 显示之前所有使用过的命令：history；

  - 显示出所有之前的命令之后可以使用 !编号 的形式重新运行；
  
- 清空屏幕：clear，也可以使用快捷键 Ctrl+L；

- 显示当前所在路径：pwd；

- 显示某一命令的可执行文件所在路径：which 命令名；

- 进入某一路径：cd 路径，直接执行 cd 则会回到当前用户的家目录；

- 查看磁盘大小：df -h

- 挂载、卸载挂载命令：mount/umount

- 分区：fdisk

- 统计当前目录下文件大小：du；

  - 只显示文件总大小：du -s；
  
- 查看文件类型：file 文件名

- 显示文件内容：cat/more 文件名；

  - 显示文件行号：cat -n；

  - cat 命令也可以同时显示多个文件内容；
  
- 分页显示文件内容：less；

  - 退出显示：q；

  - 搜索当前页内容：/；
  
- 显示文件头部内容：head，默认显示 10 行；

  - 指定显示行数：head -n 行数；
  
- 显示文件尾部内容：tail，默认显示 10 行；

  - 指定显示行数：tail -n 行数；
  
- 显示文件新增内容：tail -f；

- sed查看文件指定行数范围：sed -n ‘start_page,end_page + p’  filename

- 创建文件夹：mkdir 文件夹名；

  - 递归层级创建文件夹：mkdir -p /第一层级/第二层级/第三层级；
  
- 创建文件：touch 文件名;

- 复制文件：cp 文件名 想要复制到的路径；

  - 如果不知道文件名可以使用 * 号匹配文件
    
  - 拷贝当前目录下所有的文件：cp -r/R，会复制目录下所有文件以及子目录；
  
- 剪切文件：mv 文件名 想要移动到的路径，也可用 * 来匹配；

- 重命名文件：mv 文件名 新文件名
  - rename 文件名 新文件名
  
- 删除文件：rm 文件名；

  - 询问是否删除 rm -i；

  - 强制删除不询问：rm -f；

  - 递归删除文件夹下所有文件：rm -r；
  
- 查找一个文件所在路径：locate 文件名；

  - 查找一个关键字都有哪些文件包含：locate 关键字；

  - 此命令有一个缺陷是一个新创建的命令 24 小时之内不会被查找到；
  
- 查找命令：find；

  - 按照文件名查找：find -name 文件名；
  - 查找特定路径下是否包含文件：find /路径/路径 -name 文件名；
  - 按照文件大小查找文件：find -size 文件名 +/-文件大小；
  - 打印查找结果：find -name 文件名 -print；
  
- 管理服务：
  - systemctl start/restart/stop/reload/enable/disable 服务名.service
  - service 服务名 start/stop/restart/status
  
- 服务自启：
  - chkconfig 服务名 on/off/- - list
  
- 查看io状态：

  - iostat -d -k 2

  - ```
    参数 -d 表示，显示设备（磁盘）使用状态；-k某些使用block为单位的列强制使用Kilobytes为单位；2表示，数据显示每隔2秒刷新一次。
    ```

**Tips**：Linux 中所有文件的参数都可以组合使用：例如 ls -alht 会显示目录下所有的隐藏文件、文件详细信息、文件大小，最后一次修改时间。

#  用户与用户组命令

- 创建一个新的用户：adduser；
- 修改用户密码：passwd 用户名；
- 切换用户：su 用户名
- 删除用户：deluser 用户名；

  - 删除用户家目录：deluser 用户名 --remove -home；
- 查看当前用户所在群组：groups；

  - 查看某一用户所在群组：groups 用户名；
- 修改用户账户信息 usermod：

  - 重命名用户：usermod -l，但是用户家目录名不会改变，需要手动修改；

  - 修改用户群组：usermod -g；
- 创建一个新的群组：addgroup 群组名；
- 删除一个群组：delgroup 群组名；
- 修改一个文件的所有者和群组（需要 root 权限）：

  - 一个文件默认属于创建者和属于创建者所在群组；
- 变更一个文件所有者：chown 变更之后的用户名 文件名
    但是改变文件所有者之后文件所属的群组是不变的，需要再次变动；
  - 直接改变一个文件的所属用户和群组：chgrp 新群组 文件名
  递归改变一个目录下所有文件的所属用户和群组：chgrp -R 目录名；

**查看系统中用户**

```shell
cat /etc/passwd
```

**查看用户信息**

```shell
cat /etc/shadow
```

#  文件权限操作

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381422-c3932889-34dd-49eb-a3f9-f9e29e597ca9.png)

- d：英语 directory 的缩写，表示“目录”，就是说这是一个目录；
- l：英语 link 的缩写，表示“链接”，就是说这是一个链接；

- r：英语 read 的缩写，表示“读”，就是说可以读这个文件；
- w：英语 write 的缩写，表示“写”，就是说可以写这个文件，也就是可以修改；

- x：英语 execute 的缩写，表示“执行，运行”，就是说可以运行这个文件。

rwx 代表的就是文件的读写和执行权限，某一个文件详细信息中每一组 rwx 就代表文件的所有者和群组用户或者群组之外的用户对本文件的读写和执行权限。

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381503-6a69fb7b-a07a-483a-a81e-3d13c07394b1.png)

**root 是超级管家，它有所有权限，“只有它想不到的，没有它做不到的”。它可以读、写、运行任意文件。**

当我们需要修改一个文件的读写执行权限需要使用 chmod 命令，Linux 为文件的读写和执行权限分别分配了一个数字：

| 权限 | 数字 |
| ---- | ---- |
| r    | 4    |
| w    | 2    |
| x    | 1    |

可以用简单的加减法来分配一个用户的权限：

- 读写和执行权限：4+2+1=7；
- 只有读写权限：4+2=6；

- 只有写和执行权限：2+1=3；
- 任何权限都没有：0+0+0=0；

- 只读：4；
- 只写：2；

- 只执行：1，

当我们希望一个文件他的所属用户可以读写和执行，同一群组内用户可以读写，其他用户可以读的时候，可以用：chmod 文件名 764；

#  数据处理关命令

## 筛选数据

- 筛选数据命令：**grep 关键词 要搜索的文件**：

  - 查找被空格隔开的关键词，需将关键词用双引号引住：**grep "hello world" 文件**；

  - grep 命令默认区分大小写，忽略大小写需要加上 -i 参数：**grep -i 关键词 文件**；

  - 如果不知道要搜的是那个文件，可以递归搜索文件夹：**grep -r 关键词 文件夹**；

  - 搜索结果显示行号：**grep -n 关键词 文件**；

  - 显示搜索关键词不在的行：**grep -v 关键词 文件**；

  - 扩展正则表达式：**grep -E 正则表达式 文件**；

## 排序数据

- 对文件内容进行排序并将结果打印：**sort 文件名**；

  - sort 命令不会修改原来的文件，排序结果写入新的文件：**sort -o 新文件名 文件名**；

  - sort 默认正序排序，倒序排序加上 -r 参数：**sort -r 文件名**；

  - 随机排序：sort -R 文件名；

  - 对数字排序，sort 命令默认会将数字以字符串进行排序，比如 138 会排在 25 后面，如果想要 sort 按照数字大小进行排序可以：**sort -n 文件名**；

  - 如果文件是一列一列的，可以用 **sort -k 列数 文件名** 来指定是由那一列进行排序；

  - 如果文件每一列的分隔符不明确，可以用 **sort -t 分隔符 -k 列数 文件名** 来指定分隔符并指定要排序的列数。

- 统计文件的行数、单词数、字节数：**wc 文件名**；

  - 只统计行数：**wc -l 文件名**；

  - 只统计单词数：**wc -w 文件名**；

  - 只统计字节数：**wc -c 文件名**；

  - 只统计字符数：**wc -m 文件名**；

- 文件中有时候有重复的行，去除重复的行：**uniq 文件名**；

**Tips**：uniq 命令只能去除连续重复的行，并且也不会改变原文件内容。

- 将去除结果输出到新的文件中去：**uniq 新文件 文件名**；

- 统计每一行重复的数量：**uniq -c 文件名**；

- 只显示重复的行：**uniq -d 文件名**；
- 对文件的每一行进行剪切：**cut 文件名**；

**Tips**：cut 命令同样不会修改原文件的内容，将剪切出来的内容打印到屏幕上去。

- 保留每一行第 x 到第 x 个字符：**cut -c 2-4 文件名**；

# 流、管道符、重定向

**流**：在计算机科学中，流是时间上可用的一系列数据元素，即串行传输。

**管道**：在 Linux 中可以将两个命令使用管道符 | 进行链接起来使用，第一个命令的执行结果会成为第二个命令的参数，这种特性被称为管道。

**重定向**：在 Linux 中，我们使用键盘等设备向终端中输入命令被称为**标准输入（stdin）**，执行命令的结果被打印在屏幕上被称为**标准输出（stdout）**，如果命令执行错误，打印的错误信息被称为**标准错误输出（stderr）**。

## > 和 >> 将输出重定向到文件

 cut 命令加上两个参数：

- **-d 参数**：d 是 delimiter 的缩写，是英语“分隔符”的意思。用于指定用什么分隔符（比如逗号、分号、双引号等等）；
- **-f 参数**：f 是 field 的缩写，是英语“区域”的意思。表示剪切下用分隔符分隔的哪一块或哪几块区域。

此时 cut 命令执行结果都是输出在屏幕上的，将输出结果重定向到文件可以使用 > 符号：

```shell
cut -d , -f 3 note.csv > new.csv
```

如果 new.csv 文件不存在会新建，如果已经存在则会覆盖。

如果想要将命令执行结果追加到一个文件中，可以使用 >> 符号：

```shell
cut -d , -f 3 note.csv >> new.csv
```

这样就不会覆盖掉 new.csv 文件中的原有内容，而是会追加到文件末尾去。

## 重定向标准错误输出

\> 和 >> 符号只能将标准输出重定向到文件中，但是标准错误输出并不能被重定向，要重定向标准错误输出先要了解 Linux 中的文件标识符的概念。

**文件标识符**

文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向操作系统内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。

| 文件描述符 | 名字   | 解释         |
| ---------- | ------ | ------------ |
| 0          | stdin  | 标准输入     |
| 1          | stdout | 标准输出     |
| 2          | stderr | 标准错误输出 |

将标准错误输出重定向到文件需要文件标识符的配合，我们需要在 > 符号左面加上标准错误输出的文件标识符，也就是 2>：

```shell
cut -d , -f 3 note.csv > new.csv 2> errors.log
```

上面这条命令中有两个重定向：

- 如果 cat -d , -f 3 note.csv 命令执行成功，标准输出会被重定向到 new.csv 文件；
- 如果 cat -d , -f 3 note.csv 命令执行不成功，标准错误输出会被重定向到 errors.log 文件；

- 类似的，如果向 errors.log 追加内容，则使用 2>>。

如果要将标准输出和标准错误输出重定向到一个文件，可以使用 2>&1：

```shell
cut -d , -f 3 note.csv > new.csv 2>&1
```

这样 cat -d , -f 3 note.csv 命令无论执行是否成功，执行结果都会被重定向到 new.csv 文件中去，想要将执行结果追加到 new.csv 文件中去，可以使用下面的命令：

```shell
cut -d , -f 3 note.csv >> new.csv 2>&1
```

**管道使用示例**

使用 cut 命令剪切 note.csv 文件名字这一列，然后使用 sort 命令将名字进行排序：

```shell
cut -d , -f 1 note.csv | sort
```

cut 命令的执行结果会成为 sort 命令的参数，如果想要将执行结果重定向到文件可以配合 > 符号进行使用：

```shell
cut -d , -f 1 note.csv | sort > new.csv
```

# 系统活动和进程操作

Linux 是一个多用户系统，可以允许多个用户远程操作，如果想要查看当前 Linux 系统中有哪些用户在登录和在干什么，可以执行 w 命令：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381439-8bd7023c-ec1b-4b1d-8acc-8bee29b5351a.png)

现在系统中只有一个用户在登录，w 命令的输出信息如下：

- 第一行：

  - 13:11:03：代表当前时间；

  - up：2:53：代表当前系统已经正常运行了两小时 53 分钟，使用命令 uptime 有同样的效果，uptime 的输出和第一行一样；

  - 1 user：表示当前系统只有一个用户登录；

  - load average 0.20，0.06，0.02：表示系统负载，从左到右的三个数字分别代表：1 分钟以内的平均负载，5 分钟之内的平均负载，15 分钟之内的平均负载。

- 第二行：

  - USER：用户名（登录名）；

  - TTY：登录的终端名称；

  - FROM：用户连接到的服务器的 IP 地址（或者主机名）；

  - LOGIN@：用户连接系统的时间；

  - IDLE：用户有多久没活跃了（没运行任何命令）；

  - JCPU：该终端所有相关的进程使用的 CPU（处理器）时间，每当进程结束就停止计时，开始新的进程则会重新计时；

  - PCPU：当前进程使用的 CPU（处理器）时间；

  - WHAT：当下用户正运行的程序。

## 静态查看进程信息

查看当前用户在当前终端所运行的所有进程可以使用 ps 命令：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381555-a79cef13-df8c-4d1f-a01b-8a34e1055627.png)

- PID：进程号；
- TTY：当前进程在那个终端运行；

- TIME：进程运行了多久；
- CMD：产生进程的程序。

如果看到很多进程的 CMD 是一样的，说明这个程序产生了很多进程。当然 ps 命令只能执行当前用户在当前终端产生的进程，如果用户在别的终端产生的进程或者 root 用户产生的进程 ps 命令是不会显示的。

但是 ps 命令有很多参数，常用参数如下：

- 展示进程的详细信息：**ps -aux**；
- 列出所有用户在所有终端下产生的进程：**ps -ef**；
- 以乔木状列出所有进程：**ps -efH**；
- 列出当前用户所有的进程：**ps -u 用户名**；
- 以树形结构显示进程：**pstree**；
- 按照 CPU 使用率降序排序并分页显示：**ps -aux --sort -pcpu | less**；
- 按照内存使用率升序排序并分页显示：**ps -aux --sort +pmem | less**；
- CPU 和 内存 参数合并到一起并分页显示：**ps -aux --sort -pcpu,+pmem | less**；
- 进程status文件：cat /proc/pid号/status

## 动态查看进程信息

ps 命令的执行结果是静态的，只能显示运行 ps 命令那一时刻的进程状态，如果想要同态查看并且进程信息实时刷新可以使用 top 命令：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381477-011276a7-b943-4fa7-a90d-8e23baff2ae6.png)

top 命令无法显示全部的进程。它只显示排在前面的一些进程，这些进程是按照使用处理器的比率来排序的，从大到小（按照 %CPU 那一列的数值），最消耗处理器的进程排在最前面，如果觉得进程占用 CPU 过多可以使用 kill 命令杀死进程，用一些键盘的按键可以控制 top 的显示结果：

- q：退出 top；
- h：显示帮助文档，也就是哪些按键可以使用，按 q 回到 top 命令的主界面；

- B：大写的 B，加粗某些信息；
- f：在进程列表中添加或删除某些列，按 q 回到 top 命令的主界面。

- F：改变进程列表排序所参照的列。默认按照 %CPU 那一列来排序，按 q 回到 top 命令的主界面；
- u：依照用户来过滤显示。可以输入用户名，按回车；

- k：结束某个进程，会让你输入要结束的进程的 PID；
- s：改变刷新页面的时间。默认的，页面每隔 3 秒刷新一次。

## 杀死一个进程

杀死一个进程可以使用 kill 命令：

- 杀死一个进程：**kill 进程号**；
- 杀死多个进程：**kill 进程号 进程号 进程号**；

- 强制结束进程：**kill -9 进程号**；
- 杀死一个程序产生的所有进程：**killall 程序名**；

## 前后台进程切换

### 让命令在后台执行

在 Linux 中有的命令执行非常耗时，所以为了节省时间我们可以在同一个终端窗口同时执行好几条命令，让耗时较长的命令在后台执行即可。

让命令在后台执行只需要在命令行最后加上 & 字符即可，比如要在后台执行 /var/log 下搜索 log 关键字，可以使用下面的命令：

```shell
sudo grep -r log /var/log > output.txt & # 加 sudo 是防止出现权限不够的提示
```

运行完上面的命令之后，终端会输出：

```shell
[1] 12862
```

- [1]：代表这个进程是此终端的第一个后台进程，所以是序号 1；
- 12862：代表进程的 PID，可以用 kill 杀死这个进程。

### 将命令与终端脱离

让命令运行在后台虽然很方便，但无法命令的执行过程，这样有一个缺陷，如果后台命令没有执行完成我们就关闭了终端，这样会造成后台命令中断执行。在 Linux 中关闭终端会给这个终端中所有执行的进程发送 HUP 信号让进程中断执行，如果想要避免这种情况可以使用 nohup 命令。

nohup 命令顾名思义是让命令不受 HUP 信号的影响，只要在 nohup 之后加上要执行的命令即可，还是上面的例子：

```shell
nohup sudo grep -r log /var/log > output.txt &
```

同样会返回：[序号] 进程号，可以使用 kill 命令杀死进程。

如果命令已经在执行中，想要将命令切换为后台执行，可以使用快捷键 Ctrl + Z，以 top 命令为例，按下快捷键之后终端会打印：

```shell
[1]+  Stopped                 top # 代表 top 已经转入后台执行
```

但是 top 命令转入后台之后会处于暂停状态，可以使用 bg 命令来唤醒暂停中的进程。使用 bg 命令后，屏幕会输出：

```shell
[1]+ top & # 代表 top 命令被唤醒执行
```

bg 命令不加任何参数默认会将最近一条后台运行的命令唤醒执行，也可以根据后台进程的序号将指定的进程唤醒执行。

### 查看所有后台命令

如果想要查看所有在后台执行的进程可以使用 jobs 命令：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381929-1efdff5d-3fc9-4ac8-8122-4226198edc80.png)

jobs 命令的输出共分三列，我们逐列来说明：

1. **显示后台进程标号**：比如上例中 top 进程的标号是 1，grep 进程的标号是 2，如果还有其他后台进程，那么就会有 [3]，[4]等等。这个标号和 PID（进程号）是不一样的。这个标号只是显示当前终端下的后台进程的一个编号；
2. **显示后台进程状态**：比如 Stopped 是“停止的”的意思，Running 是“运行的”的意思。还有其他状态；

3. **命令本身**。

命令转入后台执行后，可以通过 fg 命令将后台执行的命令重新转为前台执行，与 bg 命令一样，默认将最近一条后台执行命令转为前台执行，也可以根据序号将指定的后台命令转为前台执行。

## Linux 的进程状态

Linux 的进程有 5 种状态：

1. 运行 (正在运行或在运行队列中等待)；
2. 中断 (休眠中, 受阻, 在等待某个条件的形成或接受到信号)；

1. 不可中断 (收到信号不唤醒和不可运行, 进程必须等待直到有中断发生)；
2. 僵死 (进程已终止, 但进程描述符存在, 直到父进程使用 wait4() 系统调用后释放)；

1. 停止 (进程收到 SIGSTOP, SIGSTP, SIGTIN, SIGTOU 信号后停止运行)。

在使用 ps -aux 命令查看进程详细信息时，有一列叫 STAT 这一列的标识着进程的状态：

1. D 不可中断 uninterruptible sleep (usually IO)
2. R 运行 runnable (on run queue)

1. S 中断 sleeping
2. T 停止 traced or stopped

1. Z 僵死 a defunct (“zombie”) process

# 命令的定时和延时执行

## date 定制化时间输出

date 命令会打印当前的系统时间，默认输出：

```shell
Sat Dec  5 22:56:49 CST 2020
```

我们也可以定制 date 的输出，比如只输出当前的年份信息：

```shell
date "+%H" # 2020
```

只输出当前的时间：

```shell
date "+%H:%M:%S" # 23:02:06
```

修改默认的分隔符：

```shell
date "+%H 点 %M 分 %S 秒" # 23 点 03 分 18 秒
```

定制化输出：

```shell
date "+现在是 %Y 年 %m 月 %H 时 %M 分 %S 秒" 
# 现在是 2020 年 12 月 22 时 59 分 23 秒
```

date 命令也可以修改系统时间，比如现在是 2020 年 12 月 5 日 23 点 05 分 34 秒，想要将时间修改为 2019 年 11 月 6 日 18 点 07 分 52 秒，可以使用下面的命令：

```shell
date 20191106180752 # 从左到右分别是年月日时分秒
```

## 延时执行命令

可以使用 at 命令将命令延时执行，但是 at 命令设置之后只会执行一次，想要让命令每天按时执行可以使用 crontab 命令，比如现在是 23:17，需要在 23:20 执行 touch file.txt 命令，可是使用下面的命令：

```shell
ubuntu@VM-0-4-ubuntu:/donghaojiang$ at 23:20
warning: commands will be executed using /bin/sh
at> # 在此处输入要执行的命令
```

可以在下一个 at> 提示符后继续输入，也可以按 Ctrl+D 结束命令输入：

```shell
ubuntu@VM-0-4-ubuntu:/donghaojiang$ at 23:20
warning: commands will be executed using /bin/sh
at> touch
at> <EOT>
job 3 at Sat Dec  5 23:20:00 2020
```

最后打印的这一行信息分别为：

- job：表示创建了一个任务；
- 1：是 job 的编号。表示第 1 号任务；

- at：“在…时刻”，也正是 at 命令的作用所在；
- Sat：Saturday 的缩写，代表周六；

- Dec：December 的缩写，代表 12 月；
- 5：代表 12 月 5 日；

- 23:20:00：命令执行的时刻；
- 2020：代表 2020 年。

指定命令明天 23:20 执行：

```shell
at 23:20 tomorrow
```

指定命令 12 月 19 日 23:20 执行：

```shell
at 23:20 12/19/20 # 从左到右分别是 月/日/年
```

在指定时间间隔之后执行命令：

```shell
at now +10 minutes
```

不止 minutes 这个关键字可以使用，下面列出常用的关键字：

- minutes：表示“分钟”；
- hours：表示“小时”；

- days：表示“天”；
- weeks：表示“星期”；

- months：表示“月”；
- years：表示“年”。

可以使用 atq 命令列出当前所有正在等待执行的 at 命令：

```shell
4	Sat Dec  5 23:40:00 2020 a ubuntu
```

可以根据命令的编号使用 atrm 命令将任务删除：

```shell
atrm 4
```

## 让命令休息一会再执行

在 Linux 中我们可以使用 ; 隔开两条命令：

```shell
touch file.txt;rm -rf file.txt
```

上面这条命令在执行 ; 之前的命令后会立刻执行 ; 后面的命令，两条命令之间没有任何联系，不管第一条命令成功与否，后面的命令都会执行。

使用 sleep 命令可以让第一条命令执行完成后可以暂停一段时间后在执行下一条命令：

```shell
touch file.txt ; sleep 10 ; rm -rf file.txt
```

上面的三句命令分别表示：

- touch file.txt ：创建文件 file.txt；
- sleep 10 ：暂停 10 秒；

- rm -rf file.txt ：删除 file.txt。

sleep 后面的数值默认是秒数，我们也可以指定为分钟或者是天数：

- m：minute 的缩写，表示“分钟”；
- h：hour 的缩写，表示“小时”；

- d：day 的缩写，表示“天”。

上面说到使用 ; 分隔两条命令，两条命令互不干扰，不论第一条命令执行是否成功第二条命令都会继续执行，如果想要让第一条命令执行成功之后才执行第二条命令，可以使用 && 符号将命令进行分隔：

```shell
touch file.txt && rm -rf file.txt # 第一条命令执行成功后才会执行第二条命令
```

如果想要让第一条命令执行失败之后才执行第二条命令，可以使用 || 符号将命令进行分隔：

```shell
touch file.txt || rm -rf file.txt # 第一条命令执行失败后才会执行第二条命令
```

简单说来，就是：

- &&：&& 号前的命令执行成功，才会执行后面的命令；
- ||：|| 号前的命令执行失败，才会执行后面的命令；

- ; ：不论 ; 前的命令执行成功与否，都执行 ; 后的命令。前后命令之间不相关。

## 定时执行任务

在 Linux 中可以使用 crontab 来执行定时任务，有的 Linux 发行版不会默认安装 crontab 可以使用下面的命令进行安装：

```shell
# 在 CentOS 下安装
sudo yum install vixie-cron crontabs  # 安装 Crontab
chkconfig crond on                    # 设为开机自启动
service crond start                   # 启动
# 在 Ubuntu 下安装
sudo apt install cron               # 大部分情况下 Debian 都已安装
service cron restart 或者 restart cron  # 重启 crontab
```

开启/停止/重启 crontab 服务（需要 root 权限）:

```shell
service cron start     # 启动
service cron stop      # 停止
service cron restart   # 重启动
```

使用 crontab 执行定时任务之前首先要编辑 crontab 文件，需要在 crontab 文件中编写定时任务信息，然后到了时间后 crontab 会根据 crontab 文件中的定时任务信息自动执行。

每一个用户只会有一个 crontab 文件，用户所有的定时任务都在这一个文件中汇总。

crontab 文件操作命令如下：

- crontab -e：修改 crontab 文件；
- crontab -l：显示 crontab 文件；

- crontab -r：删除 crontab 文件。

首先来查看当前用户是否有 crontab 文件，如果没有终端会打印：

```shell
no crontab for 用户名
```

然后使用 crontab -e 来修改 crontab 文件，crontab 不存在则会直接新建，如果你没有指定默认编辑器，第一次执行 crontab -e 命令会询问你使用哪个编辑器打开：

```shell
ubuntu@VM-0-4-ubuntu:~$ crontab -e
no crontab for ubuntu - using an empty one
Select an editor.  To change later, run 'select-editor'.
  1. /bin/nano        <---- easiest
  2. /usr/bin/vim.basic
  3. /usr/bin/vim.tiny
  4. /bin/ed
Choose 1-4 [1]: # 在此处输入编辑器序号选择编辑器
```

**格式：cron表达式 命令**

如果有新的定时任务可以在 crontab 文件追加即可，想要删除定时任务同理。如果想要直接清除所有的定时任务，可以使用 crontab -r 直接删除 crontab 文件即可。

# 压缩和解压缩文件

在 Linux 中可以对文件进行压缩和解压缩的操作，但是在进行文件压缩之前要先对文件进行归档操作，然后对归档后的文件进行压缩和解压缩操作：

1. 用 tar 将多个文件归档为一个总的文件，称为 archive。
2. 用 gzip 或 bzip2 命令将 archive 压缩为更小的文件。

## tar 命令归档文件

在 Linux 中一般会使用 tar 命令来归档文件：

```shell
tar -cvf 归档后的文件名 要归档的文件路径/直接跟文件名
```

tar 命令有 3 个参数：

- c：create 的缩写，表示“创建”。
- v：verbose 的缩写，表示“冗余”，会显示操作的细节。

- f：file 的缩写，表示“文件”，指定归档文件。

给 tar 命令加上 -tf 参数，不解开归档，但可以显示归档中的内容：

给 tar 命令加上 -rvf 参数，可以追加文件到归档：

tar 命令加上 -xvf 参数可以解开归档：

## gzip 和 bzip2 命令压缩文件

gzip 和 bzip2 两个命令的作用是一样的，都是对文件进行压缩，gzip 的速度快一些，但是 bzip2 的压缩率更高。使用 gzip 压缩后的文件后缀名是 .gz，使用 bzip2 压缩后的文件名是 .bz2，两个命令的格式如下：

```shell
gzip/bzip2 归档文件
```

将文件解压可以使用 gunzip 和 bunzip2 命令，格式如下：

```shell
gunzip/bunzip2 .gz/.bz2文件
```

## tar 命令直接将文件进行压缩

使用 -zcvf 参数可以直接将文件归档后使用 gzip 命令进行压缩

使用 -zxvf 参数可以直接将文件归档后使用 gunzip 命令进行解压缩

使用 -jcvf 参数可以直接将文件归档后使用 bzip2 命令进行压缩，-jxvf 命令使用 bunzip2 命令解压缩

cat、less 和 more 用来展示文件内容，但是不能显示压缩文件中的内容，展示压缩文件中的内容可以使用下面的命令：

展示使用 gzip 命令压缩的文件：

- zcat；
- zless；

- zmore；

展示使用 bzip2 命令压缩的文件：

- bcat；
- bless；

- bmore；

##  压缩和解压缩 .zip 和 .rar 文件

在 Linux 中压缩文件的后缀一般是 .gz 和 .bz2，但是有是时候需要接收 Windows 系统发送的后缀名为 .zip 和 .rar 为文件，并且有时在 Linux 系统下也要生成 .zip 和 .rar 的文件，可以使用 zip/unzip 和 rar/unrar 来将文件压缩或解压缩。

使用 zip 压缩 yasuo 目录：

```shell
zip -r yasuo.zip yasuo/ # 需要加上 -r 参数
```

使用 unzip 解压缩 .zip 文件：

```shell
unzip -r yasuo.zip
```

如果不想解开 .zip 文件，只想看其中的内容的话，可以加上 -l 参数：

```shell
unzip -l archive.zip
```

使用 unrar 解压缩 .rar 文件：

```shell
unrar l archive.rar # 需要加上 l 参数，注意没有 -
```

使用 rar 压缩 yasuo 目录：

```shell
unrar e archive.rar # 需要加上 e 参数，注意没有 -
```

# 文件下载和源码编译安装

##  下载文件

在 Linux 中我们想要下载文件，可以使用 wget 命令，比如要下载最新版本的 Python，只需要使用 wget 加上下载地址就可以了：

```shell
wget Python 解释器下载地址
```

如果中途要暂停文件的下载，可以按快捷键 Ctrl+C，也可以给 wget 命令加上 -c 参数，来继续进行被中断的下载：

```shell
wget -c Python 解释器下载地址
```

##  源码编译安装程序

在 Linux 中我们一般使用 apt 来安装程序，比如安装 MySQL 数据库：

```shell
sudo apt-get install mysql
```

但是有的软件并不在 apt 的仓库中，这个时候我们就需要下载程序的源码，通过编译源码的方式来进行安装，通过编译源码的形式来安装动态查看进程信息的 htop：

```shell
wget https://github.com/hishamhm/htop/archive/master.zip
```

下载完成之后是一个 htop-2.2.0.tar.gz 文件，进行解压缩：

```shell
tar -zxvf htop-2.2.0.tar.gz
```

解压之后会生成一个 htop-2.2.0 的目录，进入这个目录：

```shell
cd htop-2.2.0
```

目录中有一个文件叫做 conigure，运行这个文件：

```shell
./conigure
```

configure 这个程序会分析你的电脑，确认是否编译所必须的所有工具都安装了，它的执行需要些时间，因为要做不少检测。

可能会出现错误提示：

```shell
error: You may want to use --disable-unicode or install libncursesw
```

意思是：“出错啦：你也许想要用 --disable-unicode 参数或者安装 libncursesw”。

首先，我直接用第一个建议：用 --disable-unicode 参数；重新运行 configure（加上 --disable-unicode 参数）：

```shell
./configure --disable-unicode
```

用上面的命令还是不行，错误提示说：

```shell
error: missing libraries:  libncurses
```

意思是缺少 libncurses 库，来安装 libncurses 库：

```shell
sudo apt install libncursesw5-dev
```

然后，重新运行：

```shell
./configure
```

提示检测已经通过，这时执行 make 命令：

```shell
make
```

make 命令的作用是对 htop 的源码进行编译，编译完成后就可以进行 htop 的安装了：

```shell
sudo make install
```

没有问题就说明已经安装完成，运行 htop 软件：

```shell
htop
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381438-8443e26e-9435-437b-bac5-977ac1e049d3.png)

通过源码编译的方式来安装 htop 成功~~！

# Vim 编辑器

Vim 应该是 Linux 中最好用的几款编辑器之一了，一般的 Linux 发行版不会默认安装，可以通过下面的命令安装：

```shell
sudo apt install vim
```

安装完成后，在终端中输入 vim 来运行 Vim 编辑器 ，打开后的界面是这样的： ![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381445-c126de09-8c92-4c27-a744-6ff86c339be7.png)

Vim 编辑器一共分为三种模式：

## 交互模式

默认模式，不能输入文本，在键盘上按键也许会触发特定操作。

| 快捷键   | 作用                                                        |
| -------- | ----------------------------------------------------------- |
| x        | 删除字符                                                    |
| d        | 删除单词                                                    |
| dd       | 删除行                                                      |
| dw       | 删除一个单词                                                |
| d0 和 d$ | 删除从光标处到行首的所有字符 / 删除从光标处到行末的所有字符 |
| yy       | 复制行到内存中                                              |
| p        | 粘贴                                                        |
| r        | 替换一个字符                                                |
| u        | 撤销操作                                                    |
| gg       | 跳转到指定行，5gg：跳转到第 5 行                            |

## 插入模式

输入文本，文本就被插入到光标所在之处。进入这个模式最常用的方法是按字母键 i。为了退出这种模式，只需要按下 Esc 键。

![image-20220114231314326](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20220114231314326.png)

## 命令模式

可运行一些命令，为了进入这个模式，需要首先处于交互模式（Interactive Mode）下，然后按下冒号键。输入命令后，再按回车，就会执行此命令。执行命令后，就又回到交互模式了。

![image-20220114231244924](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/image-20220114231244924.png)

## Vim 常用操作

使用 Vim 打开一个文件可以使用下面的命令：

```shell
vim 文件名
```

- **w**：保存文件；
- **q**：退出文件；

- **!**：忽略错误；

三条指令可以搭配使用。比如保存文件并退出：wq，退出并忽略一切错误：q!。

## 分屏、合并、查找

这些操作同样在交互模式下进行：

| 快捷键 | 作用                                   |
| ------ | -------------------------------------- |
| /      | 按下 /（斜杠）键，那么就进入了查找模式 |
| :s     | 查找并替换                             |
| :r     | 合并文件：:r 另一个文件名              |
| :sp    | 横向分屏                               |
| :vsp   | 垂直分屏                               |
| :!     | 运行外部命令                           |

```
用法：
:s/旧字符串/新字符串：替换光标所在行的第一个匹配的字符串 
:s/旧字符串/新字符串/g：替换光标所在行的所有匹配的旧字符串为新字符串 
:#,# s/旧字符串/新字符串/g：替换文件中第 # 行到第 # 行的所有匹配旧字符为新字符 
:%s/旧字符串/新字符串/g：替换文件中所有匹配的字符串
```



## 分屏模式下的快捷键

- Ctrl + w 然后再按 Ctrl + w ：从一个 viewport 移动光标到另一个 viewport。
- Ctrl + w 然后按 j ：移动光标到下方的 viewport，如果是 h、k、l，那么是分别对应移动到如下表所示的 viewport：

| 按键              | 作用                      |
| ----------------- | ------------------------- |
| Ctrl + w 然后按 h | 移动光标到左边的 viewport |
| Ctrl + w 然后按 j | 移动光标到下边的 viewport |
| Ctrl + w 然后按 k | 移动光标到上边的 viewport |
| Ctrl + w 然后按 l | 移动光标到右边的 viewport |

- Ctrl + w 然后按 + ：扩大当前 viewport。
- Ctrl + w 然后按 - ：缩小当前 viewport。
- Ctrl + w 然后按 = ：重新均匀分配各个 viewport 的占比。
- Ctrl + w 然后按 r ：调换各个 viewport 的位置。用 R 的话是反向调换。
- Ctrl + w 然后按 q 或按 c ：关闭当前 viewport。输入 `:quit` 或 `:close` 也有一样效果。q 是 quit 的缩写，表示 “退出”。 c 是 close 的缩写，表示 “关闭”。
- Ctrl + w 然后按 o ：只保留当前所在 viewport，关闭其他 viewport。输入 `:only` 也有一样效果，o 是 only 的缩写，表示 “仅仅，只”。

# 软件包管理

## yum软件包管理

- 更新yum源：yum update
- 安装软件：yum -y install 软件名
- 清空缓存列表：yum clean packages
- 显示安装列表：yum list
- 显示安装信息：yum list 软件名
- 更新软件：yum update 软件名
- 卸载软件：yum remove nginx

## rpm软件包管理

- 安装软件：rpm -ivh 软件名
- 显示安装列表：rpm -qa
- 查询安装位置：rpm -ql 软件名
- 卸载软件：rpm -ev 软件名

# Shell 编程

Shell 是内嵌在 Linux 系统中的一个微型编程语言，可以帮我们完成很多自动化任务，例如：保存数据，监测系统的负载等等，目前为止在 Linux 中用的那些命令，之后我们也可以用在 Shell 语言中。

Shell 可以说是用户与操作系统内核沟通的桥梁：

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381474-f993a7c3-ae04-44c4-a6c8-8da580f6f482.png)

几种主流的 Shell：

- **Sh** : Bourne Shell 的缩写。可以说是目前所有 Shell 的祖先。
- **Bash** : Bourne Again Shell 的缩写， 可以看到比 Bourne Shell 多了一个 again。again 在英语中是 “又，再，此外” 的意思，说明 Bash 是 Sh 的一个进阶版本，比 Sh 更优秀。Bash 是目前大多数 Linux 发行版和苹果的 macOS 操作系统的默认 Shell。

- **Ksh** : Korn Shell 的缩写。一般在收费的 Unix 版本上比较多见，但也有免费版本的。
- **Csh** : C Shell 的缩写。 此 Shell 的语法有点类似 C 语言。

- Tcsh : Tenex C Shell 的缩写。Csh 的优化版本。
- **Zsh** : Z Shell 的缩写。比较新近的一个 Shell，集 Bash，Ksh 和 Tcsh 各家之大成。

一般 Linux 系统中会预装 Sh 和 Bash，如果想要安装其他的 Shell，如 Ksh，可以使用下面的命令：

```shell
sudo apt install ksh
```

安装完成后需要切换 Shell，才能使用 Ksh，使用下面的命令切换 Ksh：

```shell
ubuntu@VM-0-4-ubuntu:~$ chsh # 切换 Shell
Password:
Changing the login shell for ubuntu
Enter the new value, or press ENTER for the default
        Login Shell [/bin/bash]:/bin/ksh # 在此处输入其他 Shell 的路径
```

![img](https://isbut-blog.oss-cn-shenzhen.aliyuncs.com/markdown-img/1612933381641-4966e259-4f1b-41e7-a351-a2ba3b715d4b.png)

**Tips**：以下 Shell 的学习，皆以 Bash 为主。

## 创建第一个 Shell 脚本

使用 Vim 创建第一个 Shell 脚本：

```shell
vim firstShell.sh # Shell 脚本文件的后缀名是 .sh
```

在 Shell 脚本中需要制定这个脚本使用哪个 Shell 执行，如果使用 Bash 执行，需要指定：

```shell
#!/bin/bash
```

在 Shell 脚本中，注释以 # 号开头：

```shell
# 开头是注释
```

下面正式编写一个 Shell 脚本，这个脚本首先会打印当前路径，然后在当前路径下创建一个叫做 Shell1 的文件夹，并在 Shell1 文件夹中创建一个叫做 file.txt 的文件：

```shell
#!/bin/bash
pwd # 打印当前路径
mkdir Shell1 # 创建 Shell1 文件夹
cd Shell1 # 进入 Shell1 
touch file.txt # 创建 file.txt 文件
```

刚创建好的 Shell 脚本是没有执行权限的，需要给 firstShell.sh 加上执行权限：

```shell
chmod +x firstShell.sh
```

执行 firstShell.sh 脚本：

```shell
ubuntu@VM-0-4-ubuntu:~/Study_Shell$ ./firstShell.sh
/home/ubuntu/Study_Shell # 打印了当前了路径
ubuntu@VM-0-4-ubuntu:~/Study_Shell$ ls
firstShell.sh  Shell1 # 创建了 Shell1 文件夹
ubuntu@VM-0-4-ubuntu:~/Study_Shell$ cd Shell1/ 
ubuntu@VM-0-4-ubuntu:~/Study_Shell/Shell1$ ls
file.txt # 创建了 file.txt 文件
```

在 Shell 脚本的编写中难免会出现 bug，所以有时需要以调试模式来运行 Shell 脚本，在调试模式下运行 Shell 脚本会打印运行细节：

```shell
bash +x firstShell.sh
```

在运行 Shell 脚本时需要加上 ./ ，不像其他的命令一样可以直接执行，这是因为 Shell 脚本所在的路径不在环境变量中，为了方便后续执行可以把脚本所在的路径添加到环境变量：

```shell
export PATH=$PATH:/home/ubuntu/Study_Shell
```

但是这种方法只是临时的，当前的终端窗口关闭之后环境变量就失效了，如果想要设置长时间的只针对当前用户的环境变量，可以这样做：

```shell
ubuntu@VM-0-4-ubuntu:~$ vim .bashrc # 打开家目录下的 .bashrc 文件
# 在 .bashrc 文件中添加下面的语句，然后保存退出
export PATH=$PATH:/home/ubuntu/Study_Shell
ubuntu@VM-0-4-ubuntu:~$ source .bashrc # 重新加载 .bashrc 文件
```

使用下面的命令查看当前所有的环境变量：

```shell
echo $PATH
```

第一个 Shell 脚本以及后续准备工作大功告成~~！

## Shell 变量声明与输入输出

在 Shell 脚本中，定义变量的方式和 Python 一样，直接定义即可：

```shell
#!/bin/bash
message="Hello World" # 注意等号左右两侧不要有空格，不然会报语法错误
```

定义变量之后，输出变量的值使用 echo 命令：

```shell
echo $message # 输出变量之前需要加上 $ 符号
```

如果在输出内容的时候有换行符，需要给 echo 加上 -e 参数：

```shell
echo -e "first line\nsecond line"
```

在输出变量的时候也可以加上一些提示文字：

```shell
echo "message 这个变量输出的是 $message" # 注意包裹提示文字的必须要是双引号
```

将命令执行结果赋值给变量：

```shell
path = `pwd` # 将 pwd 命令的执行结果赋值给变量 path
echo $path # 输出 /home/ubuntu/Study_Shell
```

可以使用 read 命令来读取用户输入：

```shell
#!/bin/bash
read name # 脚本执行时会在此处让用户输入，用户输入的值会赋值给变量
echo "你好 $name"
```

也可以用 read 命令同时给多个变量赋值

```shell
read name age grender
echo "我的名字是 $name，年龄是 $age ,性别是 $gender"
```

在输入变量值的时候需要将多个变量的值用空格隔开，如果输入的值超过了变量的个数，read 函数会将超过的值都给最后一个变量。

在让用户输入的时候需要给用户一些提示文字，让用户知道需要输入什么内容：

```shell
read -p 'Please enter your name : ' name # 给 read 命令加上 -p 参数可以添加提示文字
echo "你好 $name !"
```

也可以限制用户输入的字符数量：

```shell
read -p 'Please enter your name (5 characters max) : ' -n 5 name # -n 参数限制输入
echo "你好 $name !"
```

限制输入时间，如果多少秒之内没有输入就不能继续输入：

```shell
read -p '请在 5 秒内输入' -t 5 code # -t 参数限制输入时间，一般默认是秒
echo -e "\nBoom !"
```

在输入一些隐私性内容时不宜让其他人看到，可以给 read 命令加上 -s 参数隐藏输入内容：

```shell
read -p '请输入密码' -s code
```

**在 Bash 中进行数学运算**

在 Bash 中**一切变量都是字符串，Bash 本身是不会操作数字的，因此也不会进行数学运算**。但是想要在 Bash 中进行数学运算还是有方法的，需要使用 let 命令：

```shell
let "a = 1"
let "b = 2"
let "c = a + b"
echo "c = $c"
```

在 Bash 中支持的运算符有以下几种：

| 运算                 | 符号 |
| -------------------- | ---- |
| 加法                 | +    |
| 减法                 | -    |
| 乘法                 | *    |
| 除法                 | /    |
| 幂（乘方）           | **   |
| 余（整数除法的余数） | %    |

下面是一些基本运算的例子：

```shell
let "a = 5 * 3"  # $a = 15
let "a = 4 ** 2" # $a = 16 (4 的平方)
let "a = 8 / 2"  # $a = 4
let "a = 10 / 3" # $a = 3 在 Shell 中整数除以整数，得到的结果还是整数。
let "a = 10 % 3" # $a = 1
```

在 Bash 中，同样支持运算的连写：

```shell
let "a = a*3"
let "a *= 3" # 两个语句的含义是等价的
```

**脚本传递参数**

Shell 脚本还可以像 Linux 的命令一样接收参数：

```shell
ubuntu@VM-0-4-ubuntu:~/Study_Shell$ firstShell.sh 参数1 参数2 参数3 ...
```

在 Shell 脚本中会自动创建变量保存这些参数：

- **$#**：总共传入的参数个数；
- **$0**：被运行的脚本的名称；

- **$1**：保存第一个参数的变量；
- **$2**：保存第二个参数的变量；

- ……。

在脚本中可以使用下面的方式去使用这些参数：

```shell
#!/bin/bash
echo "一共传入了 $# 个参数"
echo "运行的脚本是 $0"
echo "第一个参数是 $1"
echo "第二个参数是 $2"
```

**数组类型**

Bash 同样支持数组类型，新建文件 **array.sh**：

```shell
array=("v1","v2","v3","v4")
```

在 Bash 中数组的下标同样是从 0 开始，但是也有的 Shell 比如 Ksh 中数组的下标是从 1 开始的。

如果要取出数组中的一个元素，可以这样做：

```shell
echo "数组的第一个元素是：${array[0]}"
echo "数组的第二个元素是：${array[1]}"
```

取出数组中所有的元素，需要用到符号 *：

```shell
echo "数组的所有的元素是 ${array[*]}"
```

而且数组的下标不具备连续性，在其他编程语言中如果一个数组有 5 个元素，那么第 6 个元素的下标一定是 5，但是在 Bash 中我们可以指定数组元素的下标：

```shell
array[5]="v6"
echo "数组的第六个元素是：${array[5]}"
```

以上脚本输出：

```shell
数组的第一个元素是：v1
数组的第二个元素是：v2
数组的所有的元素是：v1 v2 v3 v4
数组的第六个元素是：v6
```

Shell 变量大功告成~~！

## Shell 条件控制语句

在 Shell 中也可以使用条件控制语句：

```shell
if [ 表达式 ] # 表达式两侧必须有空格，不能与中括号相连
then  # 表达式判断正确后进入 then
    做什么
elif [ 表达式 ] 
then  # elif 语句块也要写 then
    做什么
else
    做什么
fi # 条件判断结束符号
```

来看一个例子：

```shell
#!/bin/bash
name="xiaoming" # 定义变量时等号两边不可以有空格
if [ $name = "xiaoming" ]; # 条件测试中的表达式与中括号之间必须有空格
then
    echo "Hello $name"
else
    echo "who are you?"
fi # 注意要写条件判断结束语句
```

**Tips：**Shell 中的判断两个值是否相等直接用 = 号即可，不过使用 == 号也可以。

在 Shell 中判断字符串非常简单，因为 Shell 中所有的变量都是字符串：

| 条件                 | 意义                                                         |
| -------------------- | ------------------------------------------------------------ |
| $string1 = $string2  | 两个字符串是否相等。Shell 大小写敏感，因此 A 和 a 是不一样的。 |
| $string1 != $string2 | 两个字符串是否不同。                                         |
| -z $string           | 字符串 string 是否为空。                                     |
| -n $string           | 字符串 string 是否不为空。                                   |

但是有时候也需要使用判断数值的场景，来看一个例子：

```shell
#!/bin/bah
let "xiaomingAge = 18" # 使用 let 命令定义两个数字变量
let "xiaohongAge = 19"
if [ $xiaomingAge -eq  $xiaohongAge ] # 在 shell 中比较数字不能直接用比较符号
then
    echo "小明和小红同岁"
elif [ $xiaomingAge -gt  $xiaohongAge ] # 注意：elif 也要写 then
then
    echo "小明比小红大"
elif [ $xiaomingAge -lt $xiaohongAge ]
then
    echo "小明比小红小"
else
    echo "小明和小红死了，比年龄没有意义"
fi
```

在 Shell 中判断数值不能直接使用 = > < 这种比较符：

| 条件            | 意义                                                         |
| --------------- | ------------------------------------------------------------ |
| $num1 -eq $num2 | 两个数字是否相等。和判断字符串所用的符号（ = ）不一样。eq 是 equal 的缩写，是英语 “等于” 的意思。 |
| $num1 -ne $num2 | 两个数字是否不同。ne 是 not equal 的缩写，是英语 “不等于” 的意思。 |
| $num1 -lt $num2 | 数字 num1 是否小于 num2。lt 是 lower than 的缩写，是英语 “小于” 的意思。 |
| $num1 -le $num2 | 数字 num1 是否小于或等于 num2。le 是 lower or equal 的缩写，是英语 “小于或等于” 的意思。 |
| $num1 -gt $num2 | 数字 num1 是否大于 num2。gt 是 greater than 的缩写，是英语 “大于” 的意思。 |
| $num1 -ge $num2 | 数字 num1 是否大于或等于 num2。ge 是 greater or equal 的缩写，是英语 “大于或等于” 的意思。 |

在 Shell 中也可以判断文件，这一点非常的方便，可以输入一个路径，Shell 会自动判断这个路径是一个目录或者是一个文件，下面来看个案例：

```shell
# read -p "请输入一个路径：" file
if [ -d $file ]
then
    echo "file 是一个目录"
else
    echo "file 是一个文件"
fi
```

判断文件可以用以下的方法：

| 条件              | 意义                                                         |
| ----------------- | ------------------------------------------------------------ |
| -e $file          | 文件是否存在。e 是 exist 的首字母，表示 “存在”。             |
| -d $file          | 文件是否是一个目录。因为 Linux 中一切都是文件，目录也是文件的一种。d 是 directory 的首字母，表示 “目录”。 |
| -f $file          | 文件是否是一个文件。f 是 file 的首字母，表示 “文件”。        |
| -L $file          | 文件是否是一个符号链接文件。L 是 link 的首字母，表示 “链接”。 |
| -r $file          | 文件是否可读。r 是 readable 的首字母，表示 “可读的”。        |
| -w $file          | 文件是否可写。w 是 writable 的首字母，表示 “可写的”。        |
| -x $file          | 文件是否可执行。x 是 executable 的首字母，表示 “可执行的”。  |
| $file1 -nt $file2 | 文件 file1 是否比 file2 更新。nt 是 newer than 的缩写，表示 “更新的”。 |
| $file1 -ot $file2 | 文件 file1 是否比 file2 更旧。ot 是 older than 的缩写，表示 “更旧的”。 |

在 Shell 中也可以一次测试多个条件，来看个例子：

```shell
let "a = 1"
let "b = 2"
if [ $a = 1 ]  && [ $b = 2 ] # 只有两个条件都成立才会进入 then 块
then
    echo "两个条件都成立！"
else
    echo "两个条件有一个或者都不成立！"
fi
```

| 符号 | 意义                                                         |
| ---- | ------------------------------------------------------------ |
| &&   | 两个 &。表示 “逻辑与”。此符号两端的条件必须全为真，整个条件测试才为真；只要有一个不为真，整个条件测试为假。 |
| II   | 两个竖线。表示 “逻辑或”。此符号两端的条件只要有一个为真，整个条件测试就为真；只有两个都为假，整个条件测试才为假。 |

在 Shell 中也有类似 C 语言中的 Switch/Case 语句，下面来看个例子：

```shell
#!/bin/bash
case $1 in 
    "xiaoming")
        echo "我是小明"
    	;;
    "xiaohong")
        echo "我是小红"
    	;;
    "xiaogang")
        echo "我是小刚"
    	;;
    *)
        echo "我是混元形意太极门掌门人马保国"
        ;;
    esac
```

代码解释：

- **case 1 in **：判断变量1*i**n*∗∗：判断变量1 是否在下面的几种情况中；
- **"选择1")**：第一种情况；

- **;;**：第一种情况结束；
- ***)**：当变量不在任何一个情况中的时候会执行此处的语句；

- **esac**：case 语句结束符。

也可以使用 | 将结果相同的情况放在一起，来看个例子：

```shell
#!/bin/bash
animal="dog"
case $animal in
    "dog" | "cat") # 结果相同的情况使用 | 放在一起
        echo "我是家禽"
        ;;
    "tiger" | "elephant")
        echo "我是野兽"
        ;;
    *)
        echo "啥也不是"
        ;;
    esac
```

Shell 条件控制语句大功告成 ~~！

# Shell 循环控制语句

Shell 中循环主要分为三种：

- while 循环；
- for 循环；

- until 循环。

**while 循环**

while 循环的语法如下：

```shell
while [ 表达式 ] # 表达式和中括号不能相连，要有一个空格
do  # 循环体开始标识符
	echo “我是你爸爸”
done  # 循环体结束标识符
```

来看一个例子，使用 while 循环输出 0-9 这几个数字：

```shell
#!/bin/bash
let "i = 0" # 定义数字变量 i
while [ $i -lt 10 ] # 表达式判断 i 的值是否小于 10
do
    echo "$i"
    let "i += 1"
done
```

**for 循环**

for 循环语法如下，和 Python 差不多：

```shell
for i in '值1' '值2' '值3' '值4'
do
	echo "$i"
done
```

来看一个例子，使用 for 循环输出 0-9：

```shell
array=('1' '2' '3' '4' '5' '6' '7' '8' '9' '10')
for i in ${array[*]}
do
    echo $i
done
```

for 循环中也可以将命令的执行结果作为序列：

```shell
for i in `ls`
do
	echo "$i"
done
# 程序会输出当前文件夹下所有文件的名称
```

也可以使用 seq 命令来创建一个序列：

```shell
for i in `seq 1 10`
do
	echo "$i"
done
# 程序会输出 1-10 之间的数字
```

until 循环不经常使用，不再记录。

## Shell 函数使用

在 Shell 中定义函数有两种形式：

```shell
function 函数名 {
     函数体
 }
 
 函数名(){
     函数体
 }
```

例如我们定义一个打印“Hello World”的函数：

```shell
function echoHello {
    echo "Hello World"
}
# 或者
echoHello () {
    echo "Hello World"
}
echoHello # 调用函数
```

不同于一般编程语言的是：函数名后面的小括号里面不可以传递参数，在 Shell 中给函数传递参数和直接给脚本传递参数是一样的，直接在函数调用的时候传递：

```shell
addnum () {
	let "sum = $1 + $2"
    echo "$1 + $2"
}
addnum 1 2 # 打印 3
```

在 Shell 中也有变量作用域的概念，定义在函数外部的一般都是全局作用域，想要定义一个只能在函数中使用的局部作用域需要在变量的前面加上 local 关键字：

```shell
var1="global1"
var2="global2"
local_global(){
    local var2="local2"
    echo "$var1 and $var2"
}
local_global # 打印 global1 and local2
echo "$var1 and $var2" # 打印 global1 and global2
```

可以看到 local_global 函数打印了 global1 and local2，但是在函数外部打印 var1 和 var2 依然是 global1 and global2，说明这个函数调用的两个变量分别是全局作用域中的 var1 和 函数作用域中的 var2，和一般编程语言一样，在函数内部找不到这个变量会去上一级的作用域中去寻找。

如果不加 local 关键字，则会直接修改全局作用域的变量值：

```shell
var1="global1"
var2="global2"
echo "$var1 and $var2" # 打印 global1 and global1
local_global(){
    var2="local2"
    echo "$var1 and $var2"
}
local_global # 打印 global1 and local2
echo "$var1 and $var2" # 打印 global1 and local2
```

可以看到，如果不加 local 关键字，那么全局作用域中的 var2 变量的值已经被修改了。

在 Shell 中还可以使用函数将 Linux 系统命令进行重载，例如 ls 命令会列出当前目录下的所有文件，但是在当前脚本中想让 ls 命令列出 /donghaojiang 下的所有文件，可以直接使用函数进行命令重载：

```shell
ls () { # 重载命令时最好将函数名和需要重载的命令一致
    command ls /donghaojiang
} 
```

注意在要执行的命令之前加上 command 关键字，否则程序会进入无限循环状态中去。

## Shell 实战

在 /home/ubuntu/Study_Shell 目录下有一个 words.txt 文件，里面有英汉词典中所有的单词，我们需要统计其中 a-z 这 26 个英文字母一共出现了多少次，并且根据出现的次数进行排序，在执行脚本时，需要将 words.txt 当做参数传递给脚本文件：

```shell
ubuntu@VM-0-4-ubuntu:~/Study_Shell$ ls
array.sh      cycle.sh       function.sh            words.txt
condition.sh  firstShell.sh  variable_echo_read.sh
```

先来看下 words.txt 文件的内容：

```shell
aa
aaa
aah
aahed
aahing
aahs
aal
aalii
aaliis
aals
```

想要统计字母出现的次数首先要在文件中查找每一个字母，我们可以使用 grep 命令：

```shell
grep -io a words.txt
```

上面命令中 -i 参数的意思是忽略大小写，-o 参数则是匹配每一行中所有的 a ，因为每一行中有可能不止一个 a，如果不加 -o 参数，只会匹配每一行的第一个 a。

将查询结果通过 | 传递个 wc 命令来进行统计出现的次数：

```shell
grep -io a words.txt | wc -l # 只统计出现的行数
273400
```

可以看到 a 在 words.txt 文件中出现了 273400 次，有了这条命令，下面来写 Shell 脚本文件。创建一个叫做 statistics.sh 的脚本文件：

```shell
#!/bin/bash
# 因为要查询的文件会在运行脚本时当做参数传递进来，所以要判断是否传递了这个参数
if [ -z $1 ] 
then
    echo "请传入要查找的文件名"
    exit
fi
# 仅仅传递了这个参数还不够，还需要判断传递的这个文件是否是存在的
if [ !  -e $1 ]
then
    echo "对不起，您输入的文件不存在"
fi
# 如果上面的条件有一个不满足脚本会直接退出执行
# 定义一个包含 a-z 26 个字母的列表：
array=('a' 'b' 'c' 'd' 'e' 'f' 'g' 'h' 'i' 'g' 'k' 'l' 'm' 'n' 'o' 'p' 'q' 'r' 's' 't' 'u' 'v' 'w' 'x' 'y' 'z')
# 定义查找函数
chazhao (){
    for i in ${array[*]}
    do  
        # 在脚本中直接运行 Linux 命令，并将结果重定向到 result.txt 文件
        echo "字母-$i-一共出现了-`grep -io "$i" $1 | wc -l`-次" >> result.txt
    done
	
	# 对 result.txt 文件根据字母出现的次数进行升序排序
    sort -n -t - -k 4 result.txt
    
    # 排序完成后删除 result.txt 文件
    rm result.txt
}
chazhao $1
```

编写完脚本后，我们运行一下：

```shell
ubuntu@VM-0-4-ubuntu:~/Study_Shell$ statistics.sh words.txt # 将要查询的文件在运行脚本时当做参数传递，也可以直接传递其他路径下的文件
```

执行结果：

```shell
# 按照升序进行的排序
字母-q-一共出现了-5696-次
字母-x-一共出现了-10112-次
字母-z-一共出现了-13932-次
字母-w-一共出现了-22053-次
字母-k-一共出现了-25618-次
字母-v-一共出现了-32467-次
字母-f-一共出现了-38763-次
字母-b-一共出现了-61975-次
字母-y-一共出现了-67946-次
字母-g-一共出现了-80206-次
字母-g-一共出现了-80206-次
字母-h-一共出现了-86490-次
字母-m-一共出现了-99955-次
字母-d-一共出现了-108173-次
字母-p-一共出现了-109040-次
字母-u-一共出现了-126597-次
字母-c-一共出现了-146090-次
字母-l-一共出现了-187617-次
字母-t-一共出现了-223998-次
字母-r-一共出现了-237931-次
字母-o-一共出现了-241766-次
字母-n-一共出现了-242172-次
字母-s-一共出现了-245420-次
字母-a-一共出现了-273400-次
字母-i-一共出现了-297800-次
字母-e-一共出现了-363325-次
```

