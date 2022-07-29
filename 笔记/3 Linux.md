# 6.22-Linux

## linux操作系统诞生

1979 Unix7.0贝尔实验室回收版权 -> 1984谭邦宁开发Minix -> 1991 Linus在论坛上发布Linux（设计思想是兼容Unix）

所谓的Linux操作系统：运行Linux内核的操作系统

## Windows和MAC系统

## 为什么学Linux

- 服务器中使用linux操作系统占比高
- 研究linux内核代码，将学到的数据结构和设计模式落地实践
- 了解linux生态，事半功倍的学习

## linux学习方法

- 多练：难点多练习，啃难点。不要忽略屏幕输出，仔细看系统给你的每一句回复。
- 多思考，多查资料：
- 多交流，少问：
- 刻意培养自己解决问题的能力：尽可能自己解决问题

## linux学习路径

五个阶段：

1. 熟练使用linux命令行：《鸟哥的linux私房菜》、《linux就该这么学》。必须掌握
2. 使用linux进行程序设计：《unix高级编程》。必须掌握
3. 了解linux内核机制
4. 阅读内核代码，重点突破
5. 定制化内核组件，成为内核开发工程师



# 6.23-linux操作系统理解



命令行提示符：返回家目录几种：cd ~， cd 。echo ${HOME}

命令格式：命令 [选项] [选项参数] ... [参数]，   [选项]是命令执行的方式，一般以 —— 或 — 开始。

- 退出账户：ctrl + d 实质是键入一个EOF。（此处为退出linux系统的快捷键，同 exit 和 logout）

- 新建用户：useradd或adduser
- id 命令可看到用户在哪个组中
- 用户权限：d rwx rwx r-x，有d表示该文件是文件夹，无d表示该文件是文件。可通过ls -al查看。其中第一组rwx是用户权限，第二组是用户所属group权限，第三组是others权限。
- 文件结构：树形结构（windows系统和linux系统都是树形结构），但windows有多棵树，分为不同盘符；linux只有一棵树，一个根目录。
- 软件安装：apt是ubuntu的程序管理。

# 6.24-高效使用云主机

## 进入云主机

### 密码登录

- ssh shepherd@8.140.26.203
- ssh shepherd@a2：sudo vim /etc/hosts修改IP别名
- ssh shen@a1
- ssh a1
- alias x=“ssh shepherd@a2”，x：通过vim .zshrc，在尾部定义alias

### 免密登录



# 6.25-P2

## 1、terminal-shell-zsh及三者关系

### terminal

=tty（teletypewriter，电传打印机），终端，作用是提供一个命令的输入输出环境。

### shell

- shell：人机交互接口，是一种命令解析器。负责外界与linux 内核的交互。

  常见的shell：sh，bash，zsh。

  mac：下载iTerm2作为终端。

  shell修改：chsh -s /bin/zsh [user]

- shell内置命令查找的逻辑：

  - 输入echo ${PATH}，根据所得到的路径的顺序依次查找该命令。所以即使cp了该命令到别的路径，依然先找到此命令，然后只要找到就返回。

- 命令语法：命令 [选项] [选项参数] 

  head -n 5


### zsh

- 命令行命令查找修改：ctrl + s向前查找，ctrl + r向后查找，找到后光标移动到此，可做修改。
- 跳转目录：d显示前几个进入的目录，可通过数字跳转目录
- 文件别名：
  - alias -s c=vim，表示.c文件默认用vim打开



sudo apt install fortune / cowsay

管道符号：fortune | cowsay

curl wttr.in/beijing\?0



## 2、命令、程序和进程

- 命令：人和计算机交互的基本单位，人使用命令将要做什么事传递给计算机，计算机做出解析，并做出回应。

  命令语法：[主语] cp（谓语）    fileA（宾语）fileB（宾语）

  linux命令组成：

  - 命令名
  - 分隔符（空格，多个空格被视为一个空格）
  - 选项（-字母的格式）
  - 操作对象。

- 程序：每一个命令，其实对应的就是系统中的一个程序。

  计算机程序是指一组计算机执行动作或作出判断的指令，通常用某种程序设计语言编写，运行于某种目标体系结构上。

- 进程：是程序在内存中的镜像，是运行着的程序。

  ps命令查看进程状态

## 3、路径

www：你是谁、你在哪、你要做什么。明确在什么位置，即是路径。

- 绝对路径：起始点为根目录，例如/use/bin/cp

- 相对路径：起始点为当前路径，../../usr/bin/cp

- 特殊路径：

  - 家目录：~
  - 上次工作目录：cd -
  - 根目录：cd /
  - 上层目录：cd ..
  - 当前目录：cd .

- 远程路径：

  url，远程某个机器的某个目录下。

  scp：安全拷贝

  wget：

- 上下文目录：进程当前所处的目录。

## 4、软件

### 软件目录

在linux中，安装软件时，一个软件包含的内容分别会被拷贝到同级别的bin，lib，share，/etc目录下。

- bin：binary二进制文件，存放程序的可执行文件。在系统环境变量中将该路径添加进去，就可以直接执行程序。
- lib：库文件集中存放，方便共享。
- share：存放程序需要的其他资源。
- /etc：根目录的etc文件夹，系统级的权限，所有用户都可以使用。配置文件存放路径，大部分的程序的配置文件都可以在这个路径下找到。

### 在ubuntu上安装软件的逻辑：

- sudo apt update：将远端程序信息下载下来
- sudo apt install tldr：将远端的安装包下载过来。apt会处理要安装的各个软件的依赖关系。

### 软件的配置：软件启动后需要读取配置文件，完成个性化。

- 纯文本的配置文件来配置
- 配置文件可分为：全局配置文件和用户配置文件
  - 全局配置文件：一般放在/etc/目录下
  - 用户配置文件：在用户家目录下，以隐藏文件的形式存在

## 5、文件及权限

### 隐藏文件

ls -a，ls -A（除掉.和..文件）

文件类型：ls -la列表形式查看文件类型。

### 七种基本文件：烂熟于心

1. 普通文件：以-开头，regular file。细分为三种文件：
   - 纯文本文件：使用ANSII编码文件，可cat读取、vim打开
   - 二进制文件：编译好的可执行文件，不包括脚本
   - 数据文件：数据格式的文件，例如/var/log/wtmp，可通过file+文件名查看其类型。可通过命令last -f /var/log/wtmp打印出来
2. 目录文件：以d开头，directory
3. 链接文件：以l开头，link
4. 块设备：b，block，存储数据以供系统存取的接口设备，也就是硬盘。数据打包为类似集装箱，然后统一执行写入等操作。可理解为海运中集装箱运输方式，提高小效率。
5. 字符设备：c，character，串口设备，键盘，鼠标等，需要实时写入计算机的数据流。
6. 套接字：s，socket，本地主机进程与网络通信的方式，常用在网络编程中。
7. 管道：p，pip，一进一出，将标准输出作为下一步的标准输入。网络编程中。

### 文件权限

#### 文件权限：三种基本权限r，w，x

- 每个文件都有一个user和group
- user可以不在这个group中
- 除了user和group之外，其余所有用户都是others

#### 文件权限类的修改：u、g、o、a（all）

- 修改文件权限：chmod [u] [g] [o] + [r] [w] [x] [文件名]，chmod 666 [文件名]
  - chmod a+x file
  - chmod o-x file
  - chmod 755 file
  - chmod u=rwx, go=rx file

### 用户与组

linux用户：

- root：超级管理员
- 普通用户

sudo -i：临时切换到root用户

su shepherd：切换用户

id：查看用户信息

- 修改文件所属用户：

  - sudo chown [root] [shepherd] file
  - chown shepherd file #修改file的所属用户为shepherd
  - chown shepherd:kaikeba file #修改file的所属用户为shepherd，所属组为kaikeba
  - chown -R shepherd:kaikeba directory #修改目录directory及目录下的所有文件的所属组为shepherd，所属组为kaikeba

  注意：若提示不允许执行，再次输入sudo !!

- 修改文件所属组：

  sudo chgrp [root] [shepherd] file

## 6、命令系统

### zsh配置与使用

z-shell

- 修改shell

  chsh -s /bin/zsh

- zsh配置文件，zsh启动逻辑顺序：先全局，后用户。
  - ~/.zshenv
  - ~/.zprofile
  - ~/.zshrc （用户配置）   /etc/zshrc（全局配置）
  - ~/.zlogin
  - ~/.zlogout

- zsh中命令行的取值及操作

  a=123

  b=120

  let c=$a-$b

  echo $c

- zsh命令行操作快捷键
  - ctrl + b
  - ctrl + f
  - ctrl + a：调转到行首
  - ctrl + e：调转到行尾
  - ctrl + k：向后删除
  - ctrl + u：删除一行
  - ctrl + w：向前删除一个单词
  - ctrl + d
  - ctrl + c：取消改行输入，调转到下一行命令

### 通配符

- ?：代表单个字符
- *：代表几个任意字符
- [charlist] ：匹配charlist中任意单一字符，a[a-z]c.c，匹配a-z字符中的一个字符；
- [^charlist]：取反，匹配出charlist中任意单一字符。scanf("%[ ^\n ]", str)带空格的字符串的标准输入。
- [c1-c2]：匹配字母序或数字c1到c2之间任意单一字符，[0-9],[a-t]等等
- (string1 | string2 | string3)：匹配其中一个字符串，(ab | bc | cd).c匹配.c字符串前是ab、bc或cd字符串的字符串
- < num1-num2 > ：匹配任何在num1和num2之间的数字，缺省num1表示从0开始，缺省num2表示到无穷，a<0->b.c，可匹配0-无穷的数字

### 任务管理

- &：加在命令后面表示后台执行
- ；：加在命令中间表示顺序执行
- &&：连接两个命令，表示“与”，若第一个命令为假，则第二个也不执行；若第一个命令为真，第二个命令为假，则只执行第一个命令。
- ||：链接两个命令，表示“或”
- ``：命令中如果包含另一令，用该命令替换符将包含与其中的命令先执行 
- ctrl + z：命令挂起
- bg：将挂起的命令后台执行
- fg：将挂起的命令或后台执行的命令，变为前台执行（当一个命令被bg后，无法用ctrl+z将该命令挂起，此时需要先用fg转为前台运行，然后ctrl+z才能挂起）
- jobs：查看后台执行的和挂起的任务及任务编号
- %[num]将编号为num的后台任务转为前台执行

### 重定向

- '>'：重定向符号，从命令到文件的重定向./a.out > out.txt，即表示将./a.out的结果输出到out.txt中；当重复使用时，out.txt的结果会被替换掉。
- '>>'：追加符号，从命令到文件的追加。输出到out.txt的结果会被追加
- '<'：重定向符号，从文件到命令的重定向，./a.out < out.txt，表示将out.txt文件中的内容作为标准输入，输入到./out中执行
- '<<'：？，./a.out << EOF > out.txt，表示从屏幕输入，知道EOF结束，然后将结果输出到out.txt文件中
- 标准输入输出重定向
  - 标准输入重定向（STDIN，文件描述符为0）：默认从键盘输入，也可从其他文件或命令中输入
  - 标准输出重定向（STDOUT，文件描述符为1）：默认输出到屏幕
  - 错误输出重定向（STDERR，文件描述符为2）：默认输出到屏幕
- 输入重定向符号（文件作为命令的输入）
  - 命令 < 文件：将文件作为命令的标准输入
  - 命令 << 分界符：从标准输入中读入，直到遇见分界符才停止
  - 命令 < 文件1 > 文件2：将文件1作为命令的标准输入，并将标准输出到文件2
- 输出重定向符号
  - 命令 > 文件：将标准输出重定向到一个文件中（清空原有文件的数据）
  - 命令 2> 文件：将错误输出重定向到一个文件中（清空原有文件的数据）
  - 命令 >> 文件：将标准输出重定向到一个文件中（追加）
  - 命令 2> 文件：将错误输出重定向到一个文件中（追加）
  - 命令 >> 文件 2>&1、命令 &>> 文件：将标准输出与错误输出共同写入到文件中（追加）

### 管道

七种文件中的一种。作用是传递数据。

- |：将管道符号左边命令的标准输出，作为管道符号右边命令的标准输入。

- pipe和fifo（first in first out）

  有name管道和匿名管道。mkfifo a创建名字为a的管道。

  echo “1 3” > a，将echo的1和3作为标准输入重定向到管道a；./a.out < a，将管道a的输出作为标准输入到./a.out然后执行。

### 转义符

- \：反斜杠，转义，去除其后紧跟的元字符或通配符的特殊意义。
- ''：硬转义，硬引用，其内部所有的shell元字符、通配符都会被关掉。注意，硬转义中不允许出现''。例如，echo '${HOME}'输出为${HOME}
- ""：软转义，软引用，其内部只允许出现特定的shell元字符：$用于变量值替换，  `用于命令替换，\用于转义单个字符。例如，echo "${HOME}"输出为/home/shepherd为家目录

### 几个常用的命令

- touch新建文件
- echo 输出内容，echo ”hello world“，echo ${HOME}，
- 

# 6.28-P3

## 1、Linux系统信息

常用命令：

- uname：打印当前系统信息，uname -a

  cat /etc/os-release，查看系统完整信息。了解一个陌生系统的第一步。

- who，whoami，who am i：打印显示当前用户名称信息等

- date：显示或设置系统时间与日期，计算时间差等

  ```shell
  time1=  `date +%s`
  time2=`date +%s`
  let time3=time2-time1
  echo ${time3}
  ```

  ```shell
  date +"%Y-%m-%d %H:%M:%S"
  ```

- w：当前用户列表及正在执行的任务

- uptime：打印系统运行时长和平均负载

- last：显示用户最近登录信息

  lastlog：打印每个账号的最近登陆时间

- write：给其他用户发送消息，语法：write user [tty]

- wall：给其他登录用户广播信息，语法：wall “message“

  

## 2、目录与文件

### 目录

- cd切换工作目录

  cd：change directory

  cd		切换到家目录

  cd /		切换到根目录

  tree：查看目录结构，tree -d -L 1 /

  echo $0：查看当前进程

- pwd：print work directory

  ```shell
  在当前目录下编辑birth_point=`pwd`，切换到其他目录后，
  通过cd ${birth_point}也可进入之前目录。
  ```

- mkdir

  ```
  mkdir -p a/b/c/d创建多级目录
  mkdir -m 700 book增加目录权限
  ```

- rmdir

  ```
  rmdir -p删除祖先
  ```

  ### 文件与目录的管理

  

- ls：

  - -al：

- cp：

  - -i：若文件存在，询问用户
  - -r：递归复制
  - -a：pdr三个命令选项的集合
  - -p：连同文件属性一起拷贝
  - -d：若源文件为连接文件的属性，则复制连接文件的属性，权限属性等
  - -s：拷贝为软连接
  - -l：拷贝为硬连接
  - -u：源文件比目的文件新的才拷贝
  - cp file1 file2 ... dir

- rm：删除文件时，被删文件还在磁盘，只不过告诉操作系统该块内存空间不用了。下次写入时覆盖掉该块内存空间即可。因此误删时，关机找专业机构找回。

  - -i：互动模式
  - -r：递归删除（谨慎使用）
  - -f：force
  - -rf：递归、强制删除（谨慎使用），-rf /*从根目录删起（千万不要使用）

- mv：移动、重命名

  给文件重命名

  - -i：互动模式
  - -f：force
  - -u：源文件更新才会移动

- dirname，basename

### 文件内容的查阅

- cat：正向连续读

  语法cat [options] < file >

  - -A：
  - -v：可视化，列出看不见的字符
  - -E：enter，显示断行符为$
  - -T：tab，显示tab为^I
  - -b：列出行号
  - -n：列出行号，连同空行也编号

- tac：反向连续读，从最后一行开始读

- nl：给文件编行号

  语法：nl [options] < file >

  - -b：行号指定的方式：
    - -b a：相当于cat -n
    - -b t：相当于cat -b
  - -n：列出行号的表示方法
    - -n ln：行号在屏幕左边显示
    - -n rn：行号在屏幕右边显示
    - -n rz：行号在自己字段的最右边显示，前面自动填充0
  - -w < num >：行号所占位数

- more：按页查看

  语法：more < file >

  enter往下翻；b往上翻，上一页

  - /string：向下查找string关键字
  - :f ：显示文件名称和当前显示的行数
  - q：离开
  - ?：查看其它命令

- less：功能与more类似，比more更好用

  语法：less < file >

  n：继续向下查找；N：继续向上查找

- head：查看头几行

  语法：head [options] < file >

  -n num：显示前num行

  -n -num：除了后num行外其他的都显示

- tail：查看后几行

  语法：tail [options] < file >

  - -n num：显示文件后num行
  - -n +num：除了前num-1行，其他的都显示

- od：

作业：查看一个文件的第101行到120行

head -n 120 ls.log | tail -n +101

### 文件的时间

文件三个时间：

- atime：access time，内容被取用时，更新这个读取时间
- ctime：change time，权限、属性、所有者改动时，更新这个时间
- mtime：modify time，内容数据改动时，更新这个时间

修改各类时间：touch [option] file

- -a：仅修改访问时间
- -c：仅修改文件的时间，若文件不存在，不新建
- -d：修改文件日期
- -m：仅修改mtime
- -t：修改文件时间[yymmddhhmm]

### 文件的隐藏属性

lsattr：list file attributes，语法：lsattr [-adR] < file_or_dir>

- -a：打印隐藏文件的隐藏属性
- -d：如果是目录，仅打印目录的信息
- -R：递归

chattr：修改隐藏属性，语法：chattr [+-=] [option] < file_or_dir>

- A：不修改atime
- S：同步写入
- a：append，只能追加数据
- c：自动压缩、解压
- d：不会被dump程序备份
- i：不能删除、修改、建立连接
- s：文件删除时，直接从磁盘删除
- u：文件删除时，数据内容存在磁盘中

### 文件的特殊属性

- SUID：set_uid ：s：作用对象：二进制程序文件，非脚本。用户在执行该程序时获取程序所有者权限。占用x权限位置（若s为小写，说明原先有x权限；若S为大写，原先没有x权限）。
- SGID：set_gin：s：作用对象：目录和二进制程序文件。用户在该目录里，有效组变为目录所属组
- SBIT：sticky bit：t：作用对象：目录。在该目录下，用户只能删除自己创建的内容。



![微信截图_20210711205709](C:\Users\86186\Desktop\微信截图_20210711205709.png)

### 命令与文件的查找

- which：语法：which name：查找PATH路径下的所有可执行文件

  - which在什么路径下查找：echo ${PATH}可查看查找的路径
  - which查找什么文件：查找可执行文件（bin目录下的可执行文件）

- whereis：语法：whereis [option] < file_or_dir >查找特定文件

  - -b：只查找二进制文件
  - -m：只查找manual路径下的文件
  - -s：只查找source源文件
  - -u：只查找其他文件

- locate：语法：locate [option] < keyword >：模糊定位

  - 预先将文件索引记录，查找时直接从记录中查找。因此不是最新的索引记录。当新建文件时，可能查不到。此时需要用updatedb（update database）更新数据库后方能查到。
  - -i：忽略大小写
  - -r：后面可接正则表达式

- find：语法：find [path] [option] [action]：高级查找

  - 与时间相关的option：

    - -mtime n：n天前的“一天之内”修改的文件
    - -mtime +n：n天之前，不包含n，修改过的文件
    - -mtime -n：n天之内，包含n，修改过的文件
    - -newer file：比file还要新的文件。比某个file还要新的文件。

  - 与用户或用户组相关的参数：

    - -uid n：文件所属用户uid为n的用户的文件
    - -gid n：群组gid为n
    - -user name：用户名为name
    - -group name：群组名为name
    - nouser：文件所有者不存在
    - nogroup：文件所在组不存在

  - 与文件权限及名称有关的参数

    - -name filename：文件名为filename
    - -size [+-] SIZE：查找比SIZE更大或更小的文件
    - -type TYPE：f b c d l s p
    - -perm mode：mode刚好等于的文件，比如-perm 755，即查找权限等于755（rwxr-xr-x）的文件
    - -perm -mode：全部包含mode的文件

    find -exec ls -l {} \；

- 文件描述符：内核（kernel）利用文件描述符fd（file descriptor）来访问文件。

  文件描述符：

  - 0：stdio：标准输入
  - 1：stdout：标准输出
  - 3：stderror：标准错误输出

  三个宏定义，定义在open、read、write函数的头文件中：

  - 0：STDIN_FILENO
  - 1：STDOUT_FILENO
  - 2：STDERR_FILENO

![微信截图_20210712072647](C:\Users\86186\Desktop\微信截图_20210712072647.png)



### 用户管理

##### 用户管理的重要配置文件

- /etc/passwd：用户名，密码位，用户编号，归属组编号，姓名 echo $HOME， echo $SHELL
- /etc/shadow：用户名，已加密密码，密码改动信息，密码策略
- /etc/group：群组名，密码位，群组编号，组内用户
- /etc/gshadow：群主密码相关文件



##### 用户管理命令

- su：切换用户。语法：su [-lmpfc] < username >

  - -：su - TestUser：特别注意要加-，否则无法切换到TestUser1的home目录，还会停留在原用户home目录
  - -l：重新登录
  - -m | -p：不更改环境变量
  - -c comand：切换后执行命令，并退出

- sudo：临时切换为root用户。语法：sudo [-suil] < command >

  - -s：切换为root shell
  - -i：切换到root shell， 并初始化
  - -u：username|uid：执行命令的身份
  - -l：显示自己的权限

- passwd：设定用户密码。语法：passwd [-dleSxnsf] < username >

  - -d：清除密码
  - -l：锁定账户
  - -e：使密码过期
  - -S：显示密码认证信息
  - -x days：密码过期后最大使用天数
  - -n days：密码冻结后最小使用时间
  - -s：更改登录shell
  - -f：更改用户信息

- gpasswd：设定群组密码。语法：gpasswd [-adrAM] < groupname >

  - -a username：将用户加入群组
  - -d username：将用户从群组中删除
  - -r：删除密码
  - -A username：将用户设置为群组管理员
  - -M username1， username2 ... ：设置群组成员

- chsh：更改用户shell。语法：chsh [-s] < shell_path >

  - -s shell_path：shell修改为shell_path

- useradd：修改用户账号。语法：useradd [-dmMsugGnefcD] < username >

  - -d dir：制定$HOME
  - -m：自动建立$HOME
  - -s shell：这只用户登录shell
  - -u uid：设置用户编号
  - -g groupname：设定用户归属群组
  - -G groupname：设置用户归属附加群组
  - -n：不建立以用户名称为群组名称的群组
  - -e days：设置账号过期时间
  - -f days：缓冲时间，days天后关闭账号
  - -c string：设置用户备注
  - -D [表达式]：更改预设值

  /etc/login.defs 新建用户规则

  /etc/skel/ 新建用户默认文件

- usermod：新建用户。语法：usermod [-cdefgGlLsuU] < username >

  - -c string：修改备注信息
  - -d dir：修改$HOME
  - -e days：密码期限
  - -f days：密码过期后宽限的日期
  - -g groupname：修改用户所属群组
  - -G groupname：修改用户所属附加群组
  - -l username：修改用户账号名称
  - -L：锁定用户密码，使密码失效
  - -s shell：修改用户登录后所使用的shell
  - -u uid：修改用户ID
  - -U：解除密码锁定

- userdel：删除用户。语法：userdel [-r] < username >

  - -r：删除用户相关文件和目录

- id：显示用户信息。语法：id [-gGnru] < username >

  - -g：下属所属组ID
  - -G：显示附加组ID
  - -n：显示用户，所属组码，或附加群组的名称
  - -u：显示用户ID
  - -r：显示实际ID

![微信截图_20210713103205](C:\Users\86186\Desktop\微信截图_20210713103205.png)

![微信截图_20210713103553](C:\Users\86186\Desktop\微信截图_20210713103553.png)

## 3、进程管理

- free：打印系统情况和内存情况。语法：free [-bkmgotsh]

  - -b | k |m| g：以字节、KB、M、G等单位显示
  - -o：忽略缓冲区调节列
  - -t seconds：每隔seconds执行一次
  - -h：以可读形式显示

- top：显示当前系统进程情况

- dstat：实时监控磁盘、CPU等。语法：dstat [operation]

  - num1：没num1秒更新一次
  - num1 num2：每num1秒更新一次，更新num2次
  - --list：列出内部和外部插件的名称
  - --output：结果输出为csv文件

- ps：报告当前进程状态。语法：ps [-aux ef]

  - ps -aux
  - ps -ef

  ps和jobs的对比：

  - jobs是内置的shell，它显示当前shell正在管理的作业，可以提供shell内部的信息。
  - ps是一个外部命令，可以显示系统上运行的所有进程。

  ![1](D:\BaiduNetdiskWorkspace\VScode\C++_kaikeba\2 任务卡\2021.6.17-002-linux\5 作业5\1.png)

- pstree：以树状显示进程派生关系。pstree [-acGhHlnpuU]

  - -a：显示每个程序的完整指令
  - -n：使用PID排序
  - -p：显示PID
  - -u：显示用户名
  - -l：使用长列格式显示树状

- pgrep：查找进程ID。pgrep [-onlpgtu] < 进程名 >

  - -o：起始进程号
  - -n：结束进程号
  - -l：显示进程名称
  - -p pid：指定父进程
  - -p gid：指定进程组
  - -t tty：指定开启的进程终端
  - -u uid：指定uid

- kill：删除执行中的程序和工作。语法：kill [-alpsu] < PID >

  - -a：处理当前进程时，不限制命令名和进程号的对应关系
  - -l 信号ID：不加信号ID，则列出全部信号
  - -p pid：给pid的进程只打印相关进程号，而不发送任何信号
  - -s 信号ID | 信号name：指定要发出的信号
  - -u：指定用户
  - kill -9 pid：强制kill
  - kill pid

- pkill：批量按照进程名杀死进程。语法：pkill [-onpqt] < PID >

  - -o：起始pid
  - -n：结束pid
  - -p pid：指定父进程号发送信号
  - -g：指定进程组
  - -t tty：指定终端
  - pkill -9 pid



- 交换空间swap：

  - 定义：是磁盘上的一块区域，可以是一个分区，也可以是一个文件，或者是两者组合。当物理内存紧张时，Linux会将内存中不常访问的数据保存到swap上，这样系统有更多的物理内存为各个进程服务。当系统需要访问swap上存储的内容时，再将swap上的数据加载到内存中，这个过程为swap out 和swap in。

  - swap优点：
    - 大型应用程序；
    - 系统休眠时将数据保存在swap分区，系统重启时，加快启动速度；
    - 物理内存有限时可通过swap让程序运行起来。
  - swap缺点：swap是存放在磁盘上，磁盘速度比内存慢几个数量级，不同的读写swap，影响系统性能。
  - 哪些情况需要swap：
    - 当物理内存明显不够用、而且想运行程序时，swap是唯一选择。
    - 当物理内存勉强够用，可以配置swap，避免因偶尔的物理内存不够造成进程异常退出。
  - 桌面环境和服务器环境是否配置swap
    - 桌面环境：
      - 若没有配置swap，当内存用完时，内核OOM killer触发，进程被杀死，正在运行的工作没有保存；
      - 若配置了swap，虽然系统性能受影响，但还有机会去保存当前工作，因此牺牲一部分磁盘空间配置swap值得。
    - 服务器环境：当内存用完时
      - 配置了swap：服务器还能提供服务，当性能下降，直到最终处于死机状态。配置swap只能让服务苟延残喘一段时间。站在系统角度进程还在运行，但在业务角度服务已经几乎中断。
      - 没配置swap：内核的OOM killer被触发，耗内存的进程被优先杀死，这种进程一般就是业务进程，这时守护进程会自动重启该业务进程，只会造成服务中断一小会，不会出现因配置了swap而导致性能很差且服务持续中断的情况。

  

- 简答题：假设你在你的云主机上安装了MySQL数据库，请问你要如何确认它是否在正确运行，以及如果它在运行的话，应该如何结束它？

  ```
  ps //查看正在运行的进程，找到mysql进程PID
  kill -9 pid //杀死mysql进程
  ```

  

## 4、数据提取操作

- cut：切分。语法cut [-dfbc] < file >
  - -d c：以c字符分割
  - -f num：f是field（区域），显示num字段的内容[n- ; n-m; -m; m,n]可以当做索引切分
  - -b num：以字节切分，byte
  - -c num：以字符切分，char
- grep：检索。语法：grep [-cinvw] < string > < file >
  - -c：统计搜寻到的次数
  - -i：忽略大小写
  - -n：顺序输出行号
  - -v：反向输出（输出没找到的）
  - -w：匹配整个单词，而不是单词的一部分
- sort：排序。语法：sort [-fMnructk] < filr_or_stdio >
  - -f：忽略大小写
  - -M：以月份名称排序
  - -n：根据数值进行排序
  - -r：反向排序
  - -u：uniq去重
  - -c：检查文件是否有序
  - -t：分隔字符：指定排序时用的栏位分隔字符
  - -k：以哪个区间排序
  - +：排序栏位，第一栏为0，按顺序优先排序
- wc：统计字符，字数、行数。语法：wc [-lwmcL] < file_or_stdin >
  - -l：仅列出多少行
  - -w：仅列出多少字
  - -m：仅列出多少字符
  - -c：仅列出多少字节
  - -L：列出最长一行的字符长度
- uniq：去重。语法：uniq [-ic]
  - -i：忽略大小写字符的不同
  - -c：进行计数
  - -u：只输出无重复的行
- tee：双向重定向。tee [-a] file
  - -a：append追加
- split：文件切分。语法：split [-bl] < file > PREFIX
  - -b SIZE：切分为SIZE大小的文件
  - -l num：以num行为大小切分
- xargs：参数代换。语法：xargs [-pne] < command >
  - -eEOF：当xargs读到EOF时停止
  - -p：执行指令前询问
  - -n num：每次执行command时需要的参数个数
- tr：对标准输入的字符替换、压缩、删除。语法：tr [cdst] < 字符集 > <字符集>
  - -c：取代所有不属于第一字符集的字符
  - -d：删除所有属于第一字符集的字符
  - -s：将连续重复的字符以单独一个字符表示
  - -t：先删除第一字符集较第二字符集多出的字符



# shell脚本

## 0、番外

- shell，俗称壳，用来区别核。是操作系统最外面的一层，是文字操作系统与外部最主要的交互接口。
- 是一种命令解释器，接收用户输入的命令，然后调用相应的程序。
- 是一种程序设计语言，可以交互式解释和执行用户输入的命令，或自动的解释和执行预先设定好的一连串的命令。作为程序设计语言，它定义了各种变量和参数，并提供了许多在高级语言中才有的控制结构，如循环和分支等。

## 1、基础

- 一个完整的shell脚本应包含哪些内容：脚本声明、注释信息和可执行语句（即命令）
- shell脚本命令的工作方式：
  - 交互式（Interactive）：用户每输入一条命令就立即执行
  - 批处理（Batch）：由用户事先编写好一个完整的shell脚本，shell会一次性执行脚本中诸多的命令。
- .sh后缀：建议将.sh后缀加上，避免被误以为是普通文件。
- 打开/运行方式：
  - bash example.sh：用bash解释器命令直接运行shell脚本文件
  - ./example.sh：通过输入完整路径的方式执行。可能需要chmod u+x example.sh增加example.sh文件的可执行权限
- shell脚本调试
  - 方法一：bash -x 1.sh
  - 方法二：在脚本中，在需要调试的起始位置增加：set +x，在调试结束的位置增加：set -x

## 2、编写各类脚本

### 简单脚本-变量、输入输出

- 简单脚本：只执行一些预先定义好的功能，即在脚本文件中编写批量命令，批量执行各种命令

- 变量/接收用户参数的脚本：

  - 弱类型变量，变量定义时不需要指定类型。
  - declare a：a变量的声明（可选）

  - 变量之间可以使用空格间隔

  - 特殊变量：

    位置变量：

    - $0：对应脚本程序的名称。
    - $n：获取当前执行脚本的第n个参数。如果n大于9，则需要将n用大括号括起来。$1、$2等分别对应着第N个位置的参数值。这些可用于编写shell脚本时当做占位符。运行时直接传入参数即可。
    - $#：对应的是总共有几个参数。
    - $*：对应的是所有位置的参数值。
    - $@：对应的是所有位置的参数值，各参数之间用空格分开，可用于向别的函数传递参数。

    状态变量：

    - $？：对应的是显示上一次命令的执行返回值。
    - $$：取当前进程的PID
    - $!：上一个指令的PID

- 输入-read

  语法：read [-options] [variable...]

  - -a array：把输入赋值到数组array中，从索引0开始
  - -d delimiter：用字符串delimiter中的第一个字符指示输入结束，而不是一个换行符
  - -e：使用Readline来处理输入，这使得与命令行相同的方式编辑输入
  - -n num：读取num个输入字符，而不是整行
  - -p prompt：为输入显示提示信息，使用字符串prompt
  - -r：Raw mode，不把反斜杠字符解释为转义字符
  - -s：Silent mode。输入密码时不显示
  - -t seconds：超时
  - -u fd：使用文件描述符fd中的输入，而不是标准输入
  
- 输出-echo

  语法：echo string

  - echo -e "huhsfk\n"：特殊的换行符等生效
  - echo -n “sdfsnjfk”：输出后不换行

- 输出-printf

  语法：与C语言一样

### 函数

- 函数

  ```
  function __printf__(){
  	echo "$1"
  	return 234
  }
  
  __printf__ $1 $2 $3
  echo $?
  ```

- 递归函数



### 流程控制

#### test判断表达式

test表达式：[[ a ]]。两层中括号，a两边有空格

man test：test条件语句

判断/处理用户参数的脚本：

条件测试语法，即通过条件表达式的执行结果判断是否执行下一步。按照测试对象划分，条件测试语句可分为4种：

- 文件测试语句：语法：文件测试命令参数 + 逻辑运算符 + 执行的命令

  使用指定条件来判断文件是否存在或权限是否满足等情况的运算符。如果返回值为0，则文件测试存在/有权限；如果返回值为非零的值，则表示文件测试不存在/无权限。

  - -d：测试文件是否为目录类型
  - -e：测试文件是否存在
  - -f：判断是否为一般文件
  - -r：测试当前用户是否有权限读取
  - -w：测试当前用户是否有权限写入
  - -x：测试当前用户是否有权限执行

- 逻辑测试语句

  逻辑运算符：&&，||，！

  ```
  特别实例：
  [ ! $USER = root ] && echo "user" || echo "root"
  //如果当前用户为root用户，则第一步为0，第二步为0，第三步输出为root
  //如果当前用户为非root用户，则第一步为1，第二步为1，第三步输出为user
  ```

- 整数值比较语句

  只用于比较数字，不能与字符串、文件等内容一起操作。

  - -eq：等于
  - -ne：不等于
  - -gt：大于
  - -lt：小于
  - -ge：大于等于
  - -le：小于等于

- 字符串比较语句

  可用于判断测试字符串是否为空值，或两个字符串是否相同，常用来判断某个变量是否未被定义（即内容为空值）。

  - =：比较字符串内容是否相同
  - !=：比较字符串内容是否不同
  - -z：判断字符串内容是否为空



#### if条件测试语句



- 单分支结构：

  语法：

  ```
  if 条件测试语句
  	then 命令序列
  fi
  ```

- 双分支结构：

  语法：

  ```
  if 条件测试操作
  	then 命令序列1
  	else 命令序列2
  fi
  ```

- 多分支结构：

  语法：

  ```
  if 条件测试操作1
  	then 命令序列1
  elif 条件测试操作2
  	then 命令序列2
  else
  	命令序列3
  fi
  ```

#### for条件循环语句

知道要处理的数据的范围。

- 语法：

  ```
  语法一：
  for i in words; do
  	#statements
  done
  
  words:
  (1)1 2 3 4 5
  (2)a b f g t
  (3)`ls`
  (4)`seq 1 100`:1-100的序列
  
  语法二：
  for ((i = 0; i < 6; i++));do
  	#statement
  done
  ```

#### while条件循环语句

在循环结构执行前并不确定最终执行的次数。

- 语法

  ```
  while [[ test ]];do
  	#statement
  done
  ```

#### case条件测试语句

在多个范围内匹配数据

- 语法

  ```
  case 变量值 in
  模式1）
  	命令序列1
  	；；
  模式2）
  	命令序列2
  	；；
  	......
  *）
  	默认命令序列
  esac
  ```

#### until流程控制

与while循环类似

- 语法

  ```
  until [[ test ]];do
  	#statement
  done
  ```

  

### shell数组

- 语法：

  - declare -a a：声明a是个shell数组
  - name[subscript]=value：直接用下标进行赋值
    - zsh下：下标从1开始。赋值、echo时注意都是从1开始
    - bash下：下标从0开始。
  - name=(value1  value2  value3...)

- 操作

  - 输出数组内容

    ${arrar[*]}

    ${array[@]}

  - 确定数组元素个数

    ${#array[@]}

  - 找到数组的下标

    ${!array[@]}

  - 数组追加

    array+=(a b c)

  - 数组排序：sort

  - 删除数组或数组内元素

    unset array[100]，unset array



### 计划任务服务程序

- 定义：计划任务服务：在无需人为介入的情况下，在指定的时间段自动启用或停止某些服务或命令，从而实现运维的自动化。

- 分类：

  - 一次性计划任务

    只执行一次，一般用于满足临时性的工作需求。

    - at 时间：
    - at -l：查看已设置好但还未执行的一次性计划任务
    - atrm 任务序号：删除该任务

  - 长期性计划任务

    crond服务，格式：分 时 日 月 星期 命令，若有些字段没有设置，则需要使用*占位。

    在crond服务的计划任务参数中，所有命令一定要用绝对路径的方式来写。（可用whereis命令查询）

    - crontab -e：创建、编辑计划任务
    - crontab -l：查看当前计划任务
    - crontab -r：删除某条计划任务
    - crontab -u：root用户时，来编辑他人的计划任务

    ```
    0 1 * * 1-5 /usr/bin/rm -rf /temp/*
    ```

    注意：

    （1）crond服务的配置参数中，可以像shell脚本那样以#开头写上注释信息。

    （2）计划任务中的“分”字段必须有数值，不能为空或是*号，而“日”和“星期”字段不能同时使用，否则会发生冲突。



# UBUNTU



文件系统：

linux：ext 4

windows：nfts





### 分区





# 工具类

## apt

