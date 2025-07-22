## 目录结构

/	根目录

/bin	系统可执行二进制文件

/boot	内核和启动程序相关文件

/lib	库目录

/media	挂载设备（u盘，光驱..）

/mnt	让用户挂载别的文件系统

/usr	【Unix system resource】。。。一般把软件安装在`/usr/local/`下

/sbin	超级管理员的可执行程序

/proc	系统的内存映射，会保留进程运行的信息

/etc	系统软件的启动和配置文件

/dev	设备文件的所在目录

/home/user	用户的家目录

```bash
cat /etc/os-release		# 查看系统信息
uname -a				# 查看系统信息
```

## 文件属性

-[文件类型：[-][普通文件],[d][目录文件],[l][软连接],[b][块设备],[c][字符设备],[p][管道],[s][本地套接字]] rwx[归属用户的权限] rwx[归属组的权限] rwx[其他用户的权限] 1[硬链接计数] 

## 文件

`/etc/passwd` and `/etc/shadow`: 用户和密码

`/etc/os-release` and `/etc/issue`: 发行版信息

`/etc/sudoers`: sudo使用者

`/etc/hosts`：DNS表

`/var/log/syslog`：系统日志

`/proc`：把内存数据映射到文件系统。

`proc/self/fd`：文件描述符

`/proc/cpuinfo`：cpu信息

`/var/log/syslog`：系统日志

## 通配符

* `*`: 任意个
* `?`: 一个
* `[-]`: 集合内任意一个

## 重定向

`stdin`：标准输入。文件描述符是`0`。
`stdout`：标准输出。文件描述符是`1`。
`stderr`：标准错误输出。文件描述符是`2`。

`>file 2>&1`等价于`>& file`和`&> file`，表示把标准错误重定向到标准输出，标准输出重定向到文件file中。（&表示后面的1不是文件，而是文件描述符）

## 命令

1. history：查看历史命令

2. **nvidia-smi**：查看GPU状态 -L 型号

3. ls：查看文件信息 -rt 按时间排序 -R 递归显示子目录 -F文件夹加斜杠 -h 人类

   * ```bash
     ls -lh
     ```

4. jobs：查看被暂停的任务 -l 进程号

   * ```bash
     jobs -l
     ```

5. bg [id]：把任务放到后台

6. fg [id]：把任务放在前台

7. cd: 更改目录 - 上次目录 ~ 家目录
   * deng@Ubuntu:~/linux$
   * deng 代表用户
   * Ubuntu 代表机器
   * ~ 代表家目录
   * ~/linux 当前所在目录
   * $ 代表普通用户 # 代表管理员

8. pwd：显示当前目录

9. alias：设置命令别名 
   * alias ll='ls -alF'

10. man：查看命令帮助信息

11. mkdir：创建目录
   * -p：递归创建目录

   * ```bash
     mkdir -p ./dd/ddd
     ```

12. rmdir：删除（空）目录

13. which：显示命令所在位置

14. touch：创建文件（不存在），更改访问时间（存在）

15. rm：删除文件或者目录
    * -r 递归删除子目录
    * -f 强制删除（不询问）

16. cp：拷贝文件或者目录
    * -r 递归拷贝子目录

17. mv：移动、剪切

18. cat：直接显示文件内容到屏幕

19. more：分屏显示文件内容
    * f, b 分别代表forward和backward，上一页，下一页
    * 回车，逐行显示
    * 空格，逐页显示

20. less：分屏显示文件内容
    * 回车或上下键反复查看文件内容

21. head：查看文件前几行信息，默认10行
    * -n 5 显示5行

22. tail：查看文件尾，默认10hang
    * -n 指定行数
    * -f 一直跟踪到文件尾

23. wc：【word count】统计。。
    * -l 显示行数
    * -w 显示字数
    * -c 显示字节数

24. du：显示文件大小，目录占用空间
    * -h 适合人类观看

    * -s 单个目录及其子目录的和【不显示子目录】

    * --max-depth=1 | -d 1: 递归一级

    * ```bash
      du -h --max-depth=1
      du -h -d 1
      ```

25. df：显示磁盘空间
    * -h 
    * --block-size=GB 按GB为单位
    * ```bash
      df -h --block-size=GB
      ```
    * 

26. ln：创建硬链接，必须使用绝对路径
    * -s 软连接

27. unlink：删除链接

28. chmod：改变文件权限 [u|g|o|a] \[+-][r|w|x] filename

29. chown：改变用户

30. chgrp：改变组

31. find：查找文件 dir [opetion] 内容
    * -name 按名查找
    * -type 按文件类型查找 []
    * -size 按大小查找 [[+][大于]|[-][小于]]n[[b][块512B]|[c][1B]|[w][2B]|[k][1024B]|[M][1MB]|[G][1GB]]
    * -maxdepth 查找层级
    * -exec
    * -perm 按权限
    * -empty 空文件和空目录
    * -a, -o, ! 与，或，非

32. |：管道，把左边命令执行结果传给右边作为输入

    * xargs：将管道数据分块作为所执行指令的输入
      * find ./ -name "*.c" | xargs grep -nE "main"	# 把结果的每一行作为输入，多次执行管道后命令
    * exec：将管道数据一次性作为所执行指令的输入

33. grep：按内容进行查找 [option] 内容 文件
    * -n 显示行
    * -E 最新正则表达式规则
    * -v 过滤
    * -r 递归子目录
    * 正则匹配：
      * `[]`：集合中的任意一个
      * `.`：任意一个字符
      * `?`：代表前一个出现0或1次
      * `*`：代表前一个出现0到多次
      * `+`：代表前一个出现1到多次
      * `^`：后一个出现在行首 ;     `$`：前一个出现在行尾
      * `{}`：前一个重复次数
      * `\<`：后一个是单词的开头 ;    `\>`：前一个是单词的结尾
      * `()`：括号中变成一个单元
      * `|`：左或右

34. file: 查看文件类型

35. echo: 回显（自带换行）

    * -n 不带换行

36. iconv：转换编码

    * -f gb2312： from gb2312
    * -t utf-8： to utf-8

37. sort：

38. uniq：

39. nm test.o：列出函数名字

40. ld：把多个.o文件、库文件和os引代码链接起来

41. ldd：查看使用了什么库

42. ulimit -a：查看或设置用户进程的资源限制

43. umask：权限掩码

44. route：路由表 (-n)

45. netstat：展示网络信息(-an)

46. nslookup：DNS服务。根据域名获取ip

### 压缩、解压

1. *.zip
   * zip [option] 压缩包名.zip 原材料
     * -r 递归子目录
   * unzip 压缩包名.zip
     * -d 解压目录

2. *.gz | *.bzip2
   * tar [option] 压缩包名.tar.gz 原材料

   * tar [option] 压缩包名.tar.bzip2 原材料
     * -c 压缩文件
     * -x 解压文件
     * -v 显示信息
     * -f 指定压缩包名
     * -z gz格式压缩
     * -j bzip2格式压缩
     * -J xz压缩
     * -t 查看
     * --directory| -C 解压文件夹
     
   * ```bash
     tar -cvf target.tar source1 source2
     tar -xvf source.tar -C ./
     
     tar -czvf target.tar.gz source1 source2
     tar -xzvf source.tar.gz -C ./
     
     tar -cjvf target.tar.bz2 source1 source2
     tar -xjvf source.tar.bz2 -C ./
     
     tar -cJvf target.tar.xz source1 source2
     tar -xJvf source.tar.xz -C ./
     
     tar -tvf source.tar # 查看包信息
     
     for i in $(ls *.tar); do tar -xvf $i; done
     ```
     
   * 

3. *.rar
   * rar [option] 压缩包名(可以无后缀) 原材料
     * -r 递归子目录
     * a 压缩
     * x 解压

4. 7z

   ```bash
   # 解压
   7zzs x i.rar
   	# 解压目录，【紧挨着】
   	-o[dir]
   
   # 压缩
   7z a c.7z file1.txt directory1/ 
   
   	# 加密
   	-p[mypassword]
   
   	# 压缩级别，递增
   	-mx[1-9]
   ```

### 用户管理

1. useradd [option]：创建用户

   > useradd -s /bin/bash -g mygroup -d /home/deng -m deng

   1. -s 指定shell
   2. -g 指定组
   3. -d 用户家目录
   4. -m 家目录不存在时，自动创建

2. groupadd 组名：增加组

3. passwd 用户名：设置密码

4. su 用户名：切换用户

5. userdel [option] 用户名：删除用户

   * -r 连带删除家目录

6. who 

   1. -H：显示各栏位的标题信息列；
   2. -h：不要显示标题列
   3. -u：显示闲置时间，若该用户在前一分钟之内有进行任何动作，将标示成"."号，如果该用户已超过24小时没有任何动作，则标示出"old"字符串；
   4. -m：当前用户
   5. -q：只显示登入系统的帐号名称和总人数；
   6. -l
   7. w 或-T或--mesg或--message或--writable：显示用户的信息状态栏；
   8. -s：使用简短的格式来显示。
   9. -f：不要显示使用者的上线位置。

7. sudoers

   1. 把用户添加到sudoers中
   2. `/etc/sudoers`

### systemctl

```shell
# 单元：服务(.service)、挂载点(.mount)、套接字(.socket)、设备(.device)
systemctl 
	list-unit-files		# 列出所有可用单元(根据 /lib/systemd/system/ )
	list-unit			# 列出当前已经启动的 unit
	-all				# 列出所有
	--failed			# 列出所有失败单元
	--type=service		# 指定单元类型
	list-sockets		# 查看套接字
	
	start				# 立刻启动
	restart				# 立刻关闭后启动
	stop				# 立刻关闭
	reload				# 不关闭unit下,重新加载配置
	enable				# 自启动
	disable				# 取消自启动
	status				# 显示unit状态
	show				# 显示unit配置
	is-enabled			# 是否自启动
	is-active			# 是否正在运行
	kill				# 给unit进程发送kill信号
	mask				# 注销unit【无法启动】
	unmask				# 取消注销
	
```



## makefile

>  默认从第一个目标开始搜索相关依赖和目标

三要素：目标，依赖，规则命令(TAB)。

自动变量：$@, $^（全部依赖）, $<（第一个依赖）, $?（有变化的第一个依赖）。

伪目标：用不会生成的目标作为目标。make伪目标一定会执行(用`.PHONY`标出伪目标)

```makefile
# 自定义变量
变量名:=内容	
$(变量名)	# 使用

# 自动变量
gcc -c $^ -o $@	# $^表示本规则所有依赖。$@表示本规则目标
	
# 预定义变量
$(RM)	# rm -f
$(AR)	# ar
$(CC)	# cc
$(CXX)	# g++

# 匹配符
%.o:%.c		# 从上一个规则的依赖作为来源，切割循环匹配
	gcc -c $^ -o $@	-g
	
# 函数
SRCS:=$(wildcard *.c)	# 从当前目录中按*.c模式匹配文件
OBJS:=$(patsubst %.c,%.o,$(SRCS))	# 模板匹配替换
```

## 系统调用

```cpp
// man

// 改变文件权限
int chmod(const char *pathname, mode_t mode);
int ret = chmod("path", 0777);

// 获取当前工作目录
char *getcwd(char *buf, size_t size);
char *p = getcwd(NULL, 0);

// 改变当前工作目录
int chdir(const char *path);

// 创建空目录
int mkdir(const char *pathname, mode_t mode);
int stat = mkdir("path", 0777);

// 删除空目录
int rmdir(const char *pathname);

/////////////////////////////////////// 流
// 打开目录流（读写完，指针后移）
DIR *opendir(const char *name);

// 读取目录
struct dirent *readdir(DIR *dirp);	// 不用释放

// 获取当前位置
long telldir(DIR *dirp);

// 设置访问位置
void seekdir(DIR *dirp, long loc);

// 重置访问位置
void rewinddir(DIR *dirp);

// 访问文件的状态信息
int stat(const char *pathname, struct stat *statbuf);

// 从/etc/passwd/获取用户信息
struct passwd *getpwuid(uid_t uid);

// 根据组id获取组信息
struct group *getgrgid(gid_t gid);

// 把时间戳转换为日历时间
char *ctime(const time_t *timep);	// 固定格式
struct tm *localtime(const time_t *timep);	// 返回结构体

// 字符串切割
char *strtok(char *str, const char *delim); // str为空时，继续分割上一次的

////////////////////////////////////////////// 内核空间(无用户态缓冲IO)
// 打开文件。在内核空间创建文件对象并分配文件描述符
int open(const char *pathname, int flags, mode_t mode);	// 如果使用O_CREAT，则使用三参数版本,mode为文件权限

// 删除文件
int unlink(const char *pathname);

// 是否存在
int access(const char *pathname, int mode);

// 根据流获取文件描述符
int fileno(FILE *stream);
// 复制文件描述符
int dup(int oldfd);
// 减少文件对象的引用计数，为零才删除文件对象
int close(int fd);

// 读和写。把内容从内核态空间文件对象和用户态内存空间转换
// 二进制文件，写用什么类型，读就用什么类型
ssize_t read(int fd, void *buf, size_t count);	//,,最大读取长度
ssize_t write(int fd, const void *buf, size_t count);

off_t lseek(int fd, off_t offset, int whence);	// 改变文件对象读写位置

// 截断文件
int ftruncate(int fd, off_t length);	// 大->小 保留前面；小->大 补零。

/////////////////////////////////////////////
// 内存操作
void *memset(void *s, int c, size_t n);	// 设置
int memcmp(const void *s1, const void *s2, size_t n);	// 比较

// 内存映射 磁盘文件和内存之间建立映射，实现在内存随机存取
// fd必须以RDWD方式打开
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);	// char* p = (char*)mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0); (mmap前先ftruncate, lseek移动到起点)
int munmap(void *addr, size_t length);	// 释放

// 管道	进程间通信机制
// fifo 命名管道。在文件系统中存在路径。
// 读写缓冲区和暂存区。写缓冲区有数据则写阻塞；读缓冲区空则读阻塞。
int mkfifo(const char *pathname, mode_t mode);


// 加密算法
// phrase是密码，setting是附加盐值
char *crypt(const char *phrase, const char *setting);

// 获取linux用户密码等信息，需要root
struct spwd *getspnam(const char *name);
```

## IO多路复用

* `select`

```cpp
// select
// 1. 创建一个集合fd_set，监听集合 FD_ZERO 2. 设置监听集合，FD_SET/FD_CLR （每轮循环都要清空和重新设置）
// 3. 调用select，进程阻塞，OS轮询监听集合
// 4. 清空监听集合，得到就绪集合 5. 进程恢复就绪，根据就绪集合做操作,FD_ISSET
// readfds：有数据就绪 writefds：空就绪 exceptfds：自定义就绪
int select(int nfds, fd_set *readfds, fd_set *writefds,
                  fd_set *exceptfds, struct timeval *timeout);

// 缺陷：
// 1. 位图是靠数组实现，改变长度要重新编译
// 2. 大量的内核态/用户态数据拷贝
// 3. 监听和就绪耦合
// 4. 就绪集合性能低[轮询] （海量连接/少量就绪）
////////////////////////////// example //////////////////////////////
char buf[4096];
fd_set rdset;
while(1){
    FD_ZERO(&rdset);	// zero后需要重新set，标志位置零
    FD_SET(STDIN_FILENO, &rdset);	// set后标志位置一，select才可以监听。把位图拷贝到内核区
    FD_SET(fdr, &rdset);
    select(fdr+1, &rdset, NULL, NULL, NULL);	// 把就绪集合拷贝到用户区
    if(FD_ISSET(fdr, &rdset)){	// 标志位为一，则返回true
        
    }
}
////////////////////////////// example //////////////////////////////
```

* `epoll`

```cpp
// epll文件对象包含一个监听集合和一个就序集合（内核区）
// 监听集合由红黑树构成。就绪集合是一个线性表

// 创建epoll文件对象
// size以前用来限制监听集合大小，使用红黑树后，size没有用了。但是需要比0大
int epoll_create(int size);

// 设置监听集合
// epoll_event用来指定就绪的属性
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
typedef union epoll_data {
   void        *ptr;
   int          fd;
   uint32_t     u32;
   uint64_t     u64;
} epoll_data_t;
struct epoll_event {
    // 多线程用边缘触发好。
   uint32_t     events;      /* Epoll events */ // 就绪条件，读就绪|写就绪|边缘触发
   epoll_data_t data;        /* User data variable */ // 就绪以后，将要放入就序集合的信息
};

// 等待事件发生
// epoll_event* 传入就绪集合
// timeout 如果是-1则无限等待。单位毫秒
// 返回值是就绪数组的长度。（就绪数量）
int epoll_wait(int epfd, struct epoll_event *events,
                      int maxevents, int timeout);

///////////////////////////// example ////////////////////////////////////
int epfd = epoll_create(1);
int fd[2];
pipe(fd);

struct epoll_event evt;
evt.events = EPOLLIN | EPOLLET;
evt.data.fd = fd[0];
epoll_ctl(epfd, EPOLL_CTL_ADD, fd[0], &evt);
evt.events = EPOLLOUT;
evt.data.fd = fd[1];
epoll_ctl(epfd, EPOLL_CTL_DEL, fd[1], &evt);

struct epoll_event ready_evts[1024];
while(1){
    int ready_num = epoll_wait(epfd, ready_evts, sizeof(ready_evt), -1);
    for(int i = 0; i < ready_num; i++){
        if(ready_evts[i].data.fd == fd[0]){
            read();
        }else if(ready_evts[i].data.fd == fd[1]){
            write();
        }
    }
}
///////////////////////////// example ////////////////////////////////////
```

**fcntl**

```cpp
// 修改文件的属性
int fcntl(int fd, int cmd, ... /* arg */ );

// F_GETFL(void)	// 获取当前书香
// F_SETFL(int)		// 设置属性

////////////////////// example ////////////////////////
int setNonblock(int fd){
    int flag = fcntl(fd, G_GETFL);
    flag = flag | O_NONBLOCK;		// 在原文件属性的基础上加上非阻塞属性
    int ret = fcntl(fd, F_SETFL, flag);
    ERR_CHECK(ret, -1, "fcntl");
    return 0;
}
////////////////////// example ////////////////////////
```





## 进程

`ps -elf`：

`nice`：以怎样的优先级启动进程。pri：[-40~59]实时调度优先级 [60~99] 普通调度优先级 pri=80+nice

`renice`：重新设置优先级

```cpp

// 获取进程id，父进程id
pid_t getpid(void);
pid_t getppid(void);

// 调用命令
int system(const char *command);

// 创建子进程
// 用户态数据是独立的
pid_t fork(void);

// 进程执行过程中，更换代码段。夺舍（pid相同，内容不同）
// 1. 清空栈、堆
// 2. 数据段代码被新的exec换掉了
// 3. 重置pc
int execl(const char *pathname, const char *arg, ...
                       /* (char  *) NULL */);	// 可变参数。最后加一个空指针
int execv(const char *pathname, char *const argv[]);	// 数组。最后加一个空指针

// 父进程阻塞，等待子进程结束回收资源
pid_t wait(int *wstatus);	// wstatus用来存储退出状态

// 进程组
// 从命令行启动的进程会成为组长
int setpgid(pid_t pid, pid_t pgid);
pid_t getpgid(pid_t pid);

// 会话session
// 会话是进程组的集合
// 会话中第一个进程组的组长-》会话首进程
// 一个会话最多连接一个终端。前台进程组和后台进程组
// 一个会话中最多一个前台进程组，任意个后台进程组
pid_t setsid(void);	// 非组长调用。创建新会话
pid_t getsid(pid_t pid); // 获取会话id

// 守护进程 daemon。脱离了初始环境
// 所有守护进程都是孤儿
void Daemon(){
    // 1. 新建会话
    if(fork()){
        exit(0);
    }
    setsid();
    // 2. 关闭设备
    for(int i = 0; i < 64; ++i){
        close(i);
    }
    // 3. 修改环境属性
    chdir("/");
    umask(0);
}



```

## 进程间通信

* `ipcs`：查看进程间通信的信息  `ipcrm`：删除

* 管道	性能差：1. 拷贝次数多 2. 多进程，资源占用高

  * 有名管道：`mkfifo`

  * 无名管道：文件系统中没有路径。所以只能在有父子关系中使用

    * ```cpp
      ////////////////////////////////////////////////// 库函数
      // 创建子进程,父子间存在一条无名管道
      // command是子进程命令，或
      // type表示父进程是什么端口。相应子进程是用另一个端口代替标准输入输出
      FILE *popen(const char *command, const char *type);
      int pclose(FILE *stream);
      
      ////////////////////////////////////////////////// 系统调用
      // 传入数组，pipefd[0]为读端，pipefd[1]为写段
      // 先pipe，再fork。实现进程间通信
      int pipe(int pipefd[2]);
      ```
    
  * 写端关闭，读端不会就绪（缓冲区空时），再次调用read读取`0`字节

  * 写缓冲不满，写端就一直就绪。

  * 读端关闭，再`write`时，会sigpipe信号中断。

  * 非阻塞io时，`read`和`write`返回-1，`errno == EAGAIN`则可继续读写，否则出错。

* 共享内存        最快的通信机制

  * ```cpp
    // 申请（物理）共享内存
    // 0:私有共享，父子. 正数:key相同，则是同一份共享内存
    // shmflg：建议 IPC_CREAT|0600，0600为权限
    int shmget(key_t key, size_t size, int shmflg);
    
    // 申请和释放（虚拟）共享内存	attach:附着 detach:解除
    void *shmat(int shmid, const void *shmaddr, int shmflg);  // id,NULL,0
    int shmdt(const void *shmaddr);
    ```

* 信号量

* 消息队列

* 信号    软件层面的事件机制

  * 产生->阻塞->递送

  * 注册信号：更改信号的递送行为（把函数作为参数）

  * ```cpp
    // 注册信号。（不会阻塞）
    typedef void (*sighandler_t)(int);
    sighandler_t signal(int signum, sighandler_t handler);
    // 获取pending集合
    int sigpending(sigset_t *set);
    // 查看集合中有没有信号
    int sigismember(const sigset_t *set, int signum);
    // 全程屏蔽
    int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);
    // 集合操作
    int sigemptyset(sigset_t *set);	
    int sigfillset(sigset_t *set);
    int sigaddset(sigset_t *set, int signum);
    int sigdelset(sigset_t *set, int signum);
    
    
    // 不会重启低速系统调用
    int sigaction(int signum, const struct sigaction *act,
                         struct sigaction *oldact);
    
    
    // 发信号
    int kill(pid_t pid, int sig);
    int raise(int sig);
    unsigned int alarm(unsigned int seconds);	// seconds后产生alarm信号
    // 间隔定时器
    // which: 1. ITIMER_REAL 墙上时钟 2. ITIMER_VITUAL 虚拟时钟，占用cpu才计时 3. ITIMER_PRO 实用计时器,用户态cpu+系统调用
    // value：首次超时时间 interval：后续间隔时间
    int getitimer(int which, struct itimerval *curr_value);
    int setitimer(int which, const struct itimerval *new_value,
                 struct itimerval *old_value);
    ```

  * 屏蔽集合（mask）和未决集合（pending）。都是位图，保存有或没有

* 套接字

  * 对端关闭时，`read`返回`0`
  * 写缓冲慢时，`write`返回`0`
  * `read`和`write`返回-1时。
    * `errno == EINTR`：当前中断了，直接忽略
    * `errno == EAGAIN`或`EWOULDBLOCK`（win下），非阻塞socket表示数据没准备好，直接忽略。如果时阻塞socket，则是读写超时，还没有返回。（设置`SO_RCVTIMEO`和`SO_SNDTIMEO`两个属性，一般阻塞socket返回-1，关闭重连）。

  * `connect`返回-1，如果是非阻塞socket。
    * 如果`errno == EINPROGRESS`，表示正在处理中。
      * 再检测socket`写事件`
      * 通过socket选项查看错误码，如果没错误，则连接成功。
      * `getsockopt(fd, SOL_SOCKET, SO_ERROR, &err, &errLen)`

    * 如果是别的，则连接出错，要关闭重连。


## 线程

`ps -elLf`

```cpp
// 第三方库
// makefile链接时加 -lpthread

// 创建子线程
// thread：获取子线程的tid
// attr：属性（NULL默认）
// start_routine：子线程的执行函数，相当于main
// arg：start_routine的传入参数
// 返回报错。不再使用errno。使用strerrno()函数得到报错字符串。
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                          void *(*start_routine) (void *), void *arg);

// 在线程任何位置退出
void pthread_exit(void *retval);

// 等待回收指定子线程资源。（相当于wait）
// retval指向子线程的返回值
int pthread_join(pthread_t thread, void **retval);

// 线程属性 man -k pthread_attr_set。-k快速搜索，-K暴力搜索
// 线程安全。注意堆、静态区变量的使用

// 获取本线程的tid
pthread_t pthread_self(void);

// 发送线程取消请求（遇到取消点取消。1. 引发阻塞的。2. 文件。 3. printf...）
// 会带来资源泄露。尽量不使用
int pthread_cancel(pthread_t thread);
void pthread_testcancel(void); // 增加一个取消点

// 资源清理栈 	pthread_exit会自动弹，return不会
void pthread_cleanup_push(void (*routine)(void *),
                                 void *arg);
void pthread_cleanup_pop(int execute);	// 0:只弹栈不执行 整数：弹栈执行

//////////////////////// example ////////////////////////
void* routine(void* arg){
    printf("I am child thread\n");
    return NULL;
}
int main(){
    pthread_t tid; // 将用来获取子线程tid
    pthread_create(&tid,NULL,routine,NULL);
    printf("I am main thread\n");
    return 0;
}
```

**互斥锁**：		已锁则阻塞
原子操作。线程共享。标志位
未锁则加锁。有锁则阻塞。

```cpp
// 创建和初始化锁
// attr：属性。普通锁、检错锁(检测同一线程多次加锁)、可重入锁、默认锁
int pthread_mutex_init(pthread_mutex_t *restrict mutex,
           const pthread_mutexattr_t *restrict attr);	// 动态初始化
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;	// 静态初始化

// 加锁、解锁
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);

// 非阻塞加锁。已锁报错
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);

// 属性操作。
int pthread_mutexattr_init(pthread_mutexattr_t *attr); // 初始化锁属性
int pthread_mutexattr_destroy(pthread_mutexattr_t *attr); // 销毁锁属性
int pthread_mutexattr_settype(pthread_mutexattr_t *attr, int type);	// 设置锁
```

**自旋锁** 		已锁则while(1)

```cpp
int pthread_spin_init(pthread_spinlock_t *lock, int pshared);
int pthread_spin_destroy(pthread_spinlock_t *lock);
```

**读写锁**

```cpp
int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock);
int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock);
```

**条件变量**	不占用cpu的阻塞

```cpp
// wait：后事件(阻塞等待)  signal：先事件(唤醒等待)
// 一定要配合条件flag
// wait需要锁条件flag来保护条件。所以wait需要一个锁

// 条件变量初始化
int pthread_cond_destroy(pthread_cond_t *cond);
int pthread_cond_init(pthread_cond_t *restrict cond,
			const pthread_condattr_t *restrict attr);
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

// 唤醒和等待
int pthread_cond_broadcast(pthread_cond_t *cond);	// 全唤醒
int pthread_cond_signal(pthread_cond_t *cond);	// 唤醒一个
int pthread_cond_wait(pthread_cond_t *restrict cond,	// 加入阻塞队列。醒来第一时间加锁
           pthread_mutex_t *restrict mutex);

// 具有最长等待时间的wait。（一般用来实现高精度定时器）
int pthread_cond_timedwait(pthread_cond_t *restrict cond,
           pthread_mutex_t *restrict mutex,
           const struct timespec *restrict abstime);
struct timespec {
    time_t tv_sec;		/* seconds */
    long tv_nsec;		/* nanoseconds */
};

////////////////////////////// example//////////
int main(){
    pthread_mutex_lock(&mutex);
    while(flag != 0){
        pthread_cond_wait(&cond, &mutex);
    }
    pthread_mutex_unlock(&mutex);
}
int sub(){
    pthread_mutex_lock(&mutex);
    flag = 1;
    pthread_mutex_unlock(&mutex);
    pthread_cond_signal(&cond);
}
```

**单例线程安全**

```cpp
// 注册函数，在程序结束时调用一次
int atexit(void (*function)(void));

// 多线程下，init_routine只调用一次
int pthread_once(pthread_once_t *once_control,
           void (*init_routine)(void));
```



## 进程池和线程池

> 池：提前申请好资源
>
> 进程池、线程池、连接池、内存池

```cpp
// 用信号机制实现进程池的有序退出
// 使用10号信号，SIGUSR1（用户自定义信号）
// 为了避免太多全局变量，使用异步拉起同步
int exit_pipe[2];
void handler(int sig){
    write(exit_pipe[1], "1", 1)
}
int main(){
    // epoll ...
    if(ready_evts[i].data.fd == exit_pipe[0]){
        for(int j = 0; j < worker_num; j++){
            kill(child[j].pid, SIGKILL);
        }
    }
}

// 多线程和信号机制是有冲突的
// 创建
```



## 网络

**KCP协议**:游戏使用

wireshark：解析和过滤包
Ncap：抓包工具（win）
tcpdump：抓包工具（linux）[-i 指定网卡] [-w 把抓包内容写入文件] [port 删选端口]

**解决网络问题的一般步骤：**

1. netstat -an
2. 抓包
3. 用wireshark分析抓到的包

* BSD. Berkeley socket库已经实现传输层以下的所有层。

> 网络字节序：大端存储	
>
> 主机字节序：主机设备，intel/AMD x86 小端
>
> ```cpp
> // h host; n net; l long; s short
> uint32_t htonl(uint32_t hostlong);
> uint16_t htons(uint16_t hostshort);
> uint32_t ntohl(uint32_t netlong);
> uint16_t ntohs(uint16_t netshort);
> ```

```cpp
// 相当于ipv4和ipv6的父类。
struct sockaddr_in {
   sa_family_t    sin_family; /* address family: AF_INET */
   in_port_t      sin_port;   /* port in network byte order */
   struct in_addr sin_addr;   /* internet address */
};
/* Internet address. */
struct in_addr {
   uint32_t       s_addr;     /* address in network byte order */
};

// ip地址转换
// in_addr_t等于uint32_t。char*表示点分十进制。
int inet_aton(const char *cp, struct in_addr *inp);
in_addr_t inet_addr(const char *cp);
in_addr_t inet_network(const char *cp);
char *inet_ntoa(struct in_addr in);
// 把128bit或32bit的大端地址转换为人类可读的字符串。（除了ipv4还可以转ipv6）
const char *inet_ntop(int af, const void *src,
                             char *dst, socklen_t size);

//////////////////////////// example ///////////////////////////
struct scokaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(0x1234);
inet_aton("192.168.224.1", &addr.sin_addr);
addr.sin_addr.s_addr = inet("192.168.224.1");
//////////////////////////// example ///////////////////////////

// 访问DNS服务器，根据域名获取ip
// 需要联网。联网报错用herror()
struct hostent *gethostbyname(const char *name);
struct hostent {
   char  *h_name;            /* official name of host */
   char **h_aliases;         /* alias list */
   int    h_addrtype;        /* host address type */
   int    h_length;          /* length of address */
   // 获取的是32bit大端,不是字符串
   char **h_addr_list;       /* list of addresses */
}
```

```cpp
////////////////// socket ///////////////////
// domain: AF_INET	type: SOCK_STREAM (TCP)	protocol: 0 (自动选择)
// domain: AF_INET	type: SOCK_DGRAM (UDP)	protocol: 0 (自动选择)
// socket会创建一个socket文件对象（拥有读和写缓冲区）并分配fd。
int socket(int domain, int type, int protocol);

// 服务端socket必须bind ip和端口。客户端不建议（连接会是用一条连接）
// sockaddr* 是公共类型。需要强转。
int bind(int sockfd, const struct sockaddr *addr,
                socklen_t addrlen);

// 客户端发送连接请求。sockaddr* 是服务端地址
int connect(int sockfd, const struct sockaddr *addr,
                   socklen_t addrlen);

// 服务端开启监听。
// 开启后，更改socket文件对象的内部数据结构【监听socket】，把发送缓冲区和接受缓冲区改为两个队列(半连接队列和全连接队列)。队列存储连接的信息
// 开启后，服务端socket对象只能用来建立连接，不能用来收发数据。
int listen(int sockfd, int backlog);

// 取出连接，构造一个新的socket对象【通信socket】,实现全双工通信。（如果没有连接则阻塞）
// sockaddr* 和 socklen_t* 可NULL。可用来获取客户端地址。（addrlen得指向非0长度）
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);

////////////////////// TCP /////////////////////////////
// 将数据拷贝到发送缓冲区。真正的发送由内核协议栈完成(用内核协议实现可靠传输和分片限制报文段数据长度)
// flags: MSG_NOSIGNAL -- 不产生SIGPIPE信号
ssize_t send(int sockfd, const void *buf, size_t len, int flags);
// 将数据从接收缓冲区拷贝到用户区buf。
// flags: MSG_NOWAIT -- 临时非阻塞
//        MSG_WAITALL -- 等待接收len所有数据（用于解决半包问题）
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
////////////////////// TCP /////////////////////////////

////////////////////// UDP /////////////////////////////
// 收发数据是有边界的
// 
ssize_t sendto(int sockfd, const void *buf, size_t len, int flags,
              const struct sockaddr *dest_addr, socklen_t addrlen);
// （可获取发送方ip和端口）
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags,
                        struct sockaddr *src_addr, socklen_t *addrlen);
////////////////////// UDP /////////////////////////////

////////////////////// 零拷贝 /////////////////////////////
// 通过mmap方式实现内核区之间的拷贝。最大2G
// in_fd需要能够支持mmap（磁盘文件）
// out_fd最好是socket
ssize_t sendfile(int out_fd, int in_fd, off_t *offset, size_t count);
//////////////////////  /////////////////////////////

////////////////////// 高级的收发，可以发送接收内核数据
// 网络间
ssize_t sendmsg(int sockfd, const struct msghdr *msg, int flags);
ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);
struct iovec {                    /* Scatter/gather array items */
   void  *iov_base;              /* Starting address */
   size_t iov_len;               /* Number of bytes to transfer */
};// 描述离散内存区域的碎片
struct msghdr {
   void         *msg_name;       /* Optional address */ // NULL
   socklen_t     msg_namelen;    /* Size of address */ // 0
   struct iovec *msg_iov;        /* Scatter/gather array */// 正文 数组首地址
   size_t        msg_iovlen;     /* # elements in msg_iov */ // 数组长度
   void         *msg_control;    /* Ancillary data, see below */ // 
   size_t        msg_controllen; /* Ancillary data buffer len */
   int           msg_flags;      /* Flags on received message */
};  
struct cmsghdr {
   size_t cmsg_len;    /* Data byte count, including header
                          (type is socklen_t in POSIX) */
   int    cmsg_level;  /* Originating protocol */	// SOL_SOCKET
   int    cmsg_type;   /* Protocol-specific type */	// SCM_RIGHTS
/* followed by
  unsigned char cmsg_data[]; */
}; // man cmsg

// 和pipe功能一样
// 是socket，全双工。sv[0]、sv[1]都是既可读又可写
// domain: AF_LOCAL (本地协议) 
int socketpair(int domain, int type, int protocol, int sv[2]);
//////////////////////

// 设置socket选项。level: SOL_SOCKET
// man 7 socket。REUSEADDR使bind重用地址端口
int setsockopt(int sockfd, int level, int optname,
                      const void *optval, socklen_t optlen);
///////////////////// example /////////////////////
int reuse = 1;
setsockopt(fd, SOL_SOCKET, SO_REUSEADDR, &reuse, sizeof(reuse));
///////////////////// example /////////////////////
```

## mysql

```mysql
# 数据类型
# 数值类型：整数、定点数、浮点数
# 字符串类型：时间日期、字符串、二进制(BLOB[大的二进制，图片、音乐]、TEXT[大的文本，网页])

-lmysqlclient
```

**SQL**

```mysql
# 取消之前的输入
/c

# 显示所有数据库、表
show databases | tables;

# 切换database，每次登录都要执行（非SQL语句）
use basename

# 展示当前的database
select database();

# 创建数据库
create database name;

#删除数据库
drop database name;

# 查询表结构
describe|desc name;

#
create table name (item1 varchar(20), item2 date, 
                   primary key(item1,item2),
                  index idx_name(col_name,));

# 显示创建命令
show create table name;

################### 行
# 插入
insert into Tname values('col1','col2');
insert into Tname (feild1, feild3) values('val1','val2');

# 修改
update tName set col1='val1' where 

# 删
delete from tName where	# 可恢复
truncate tName # 不可恢复
################### 行

################### 列
# 增
alter table tName add (col3 varchar(1), col4 char(2));
# 删
alter table tName drop col3;
# 改
alter table tName change old new varchar(1);
################### 列
#
# % 任意字符串。- 任意一个字符
#

############## 事务 【考锁实现】 ##############
# 启动事务
start transaction; | begin;(mysql)
# 回滚（放弃）
rollback;
# 提交
commit;
# 存储点（回滚到）
savepoint tag
rollback to tag	# 仍然在事务中
############## 事务 【考锁实现】 ##############

################### 隔离级别 ###################
# 问题： 脏写(写未提交)	->	脏读(读未提交)	->	不可重复读	->	幻读 
# 解决：  |-> 不能同时写 -> 读已提交 -> 备份或加锁 -> 全表备份或加锁 -> 可串行化
# 查看隔离级别
select @@transaction_isolation
# 设置隔离级别
# read uncommitted、read committed、repeatable read【使用MVCC多版本并发控制机制部分解决幻读】
# 
set session transaction isolation level read uncommitted;
################### 隔离级别 ###################

# 约束
# 域约束：NOT NUL,
# 实体约束：主键
# 参照约束：外键

################### innoDB ###################
# 主键索引
# 唯一键索引
# 查看索引
create table ... key idx_name (col);

show index from table_name;

create index idx_name on table_name(col_name);

drop index idx_name on table_name;

# 创建表时，以主键或唯一键创建聚簇索引【存储表本身】
# 之后创建的其他索引都是非聚簇索引【索引到的结果是主键】
# 聚簇索引：B|B+树叶节点包含表数据本身
# 非聚簇索引：B|B+树叶节点只包含指向表数据的指针或主键

# 覆盖索引是不回表的查询（1. 以主键作条件 2. 只查主键 3. 只查索引关注到的列）

# 索引失效：
	# 1. 对索引列做运算 
	# 2. 函数调用
	# 3. 使用or时，不是所有都有索引
	# 4. 去掉联合索引的最左索引
	# 5. 使用<>，主键索引可能失效
	# 6. 触发类型转换
	# 7. 使用like时%在开头
################### innoDB ###################


################### 锁 #######################
# 默认上行级锁
# 读锁和写锁不能同时上
# 范围上锁，可能是间隔锁，
# 上读锁
... lock in share mode;
# 上写锁 (悲观锁，)
... for update;
################### 锁 #######################

explain # 查看SQL命令的执行计划

```

**CAPI**

> 每个线程单独init/real_connect和close

```cpp
//////////////////////////// 建立和断开连接 ////////////////////////////
// 分配内存和初始化
MYSQL* mysql_init(MYSQL* mysql); // 线程不安全，需要加锁

// 建立连接
MYSQL *mysql_real_connect(MYSQL *mysql, const char *host, const char *user, const char *passwd, const char *db, unsigned int port, const char *unix_socket, unsigned long clientflag);

// 断开连接
void mysql_close(MYSQL *mysql);
//////////////////////////// 建立和断开连接 ////////////////////////////

//////////////////////////// SQL语句 ////////////////////////////
// 发送SQL
int mysql_query(MYSQL *mysql, const char *query);

// 存储query语句查询结果
MYSQL_RES *mysql_store_result(MYSQL *mysql);	// 使用了读操作SQL语句时必用
void mysql_free_result(MYSQL_RES *result);	// 释放查询结果内存

// 从结果中取一行
MYSQL_ROW mysql_fetch_row(MYSQL_RES *result);

// 获取列数和行数
unsigned int mysql_num_fields(MYSQL_RES *result);
my_ulonglong mysql_num_rows(MYSQL_RES *result);
//////////////////////////// SQL语句 ////////////////////////////

//////////////////////////// 报错信息 ////////////////////////////
const char* mysql_error(MYSQL* mysql);
//////////////////////////// 报错信息 ////////////////////////////
```



## 如何写"大"项目

1. 先搞好代码文件布局，弄清楚编译链接关系，写好makefile
2. 设计数据结构（.h）。
3. 设计相关函数
4. 单元测试

## 哈希算法

```shell
# md5
md5sum file

# SHA1
sha1sum file
```

```cpp
#include <openssl/md5.h>
#include <openssl/sha1.h>
int main(){
    unsigned char* md5_key = MD5("haha");
    
    SHA_CTX ctx;
    SHA1_Init(&ctx);
    while(/*...*/){
        // ...
        SHA1_Update(&ctx, buf, size);
    }
    unsigned char sha1[20] = {0}	// 存储哈希值的二进制形式
    SHA1_Final(sha1, &ctx);
    
}
```



## 日志

```cpp
// linux自带的系统日志
void openlog(const char *ident, int option, int facility);
void syslog(int priority, const char *format, ...);
void closelog(void);

// 编译器自带宏
// __LINE__ 行号
// __FUNCTION__ 函数名
// __FILE__ 文件名
```



## screen | nohup

1. ```bash
   screen -ls # 列出所有终端
   screen -R[r] name #创建或进入终端，R如果不存在会创建，r不会
   ctrl+a d # 回到主终端
   exit # 退出并终止终端
   screen -R [pid/Name] -X quit # 终止终端
   screen -wipe #清理那些dead的会话
   ```

2. ```bash
   mkdir log
   nohup python -u train.py > ./log/*.log 2>&1 & 
   # 吧程序放在后台执行，断开ssh依然运行
   # -u （unbuffered，不缓存）这个参数加在python的后面，可以实时查看输出，而不用等把一段日志文件写入log.txt后才能查看
   # 2>&1 是将标准输出与错误输出共同写入到文件中，这里的标准输出已经重定向到了log.file文件，即将标准出错也输出到out.file文件中。2与>结合代表错误重定向，而1则代表错误重定向到一个文件1，而不代表标准输出；换成2>&1，&与1结合就代表标准输出了，就变成错误重定向到标准输出。
   # 最后一个& 是让该命令在后台执行。
   tail -f log.txt # 实时查看文件的尾部数据。-f 查看正在改变的日志文件
   ```

##  http.server

```bash
#不安全，不能用于生产环境
python -m http.server # 默认端口服务器
python -m http.server 9909 # 自定义端口
python -m http.server -d /home/usr/ # 指定目录
```

## pip

1. `pip cache purge`: 清除下载缓存
2. `pip cache remove package-name`:清楚指定安装包缓存

## git

[手把手教你入门Git --- Git使用指南（Linux）_linux 用git-CSDN博客](https://blog.csdn.net/weixin_44966641/article/details/119791118)

## vim

### 搜索

````bash
:%s/old_text/new_text/gc # %文件范围 s替换 g全局替换 c选择
/text # 搜索

:7,10s/old/new/g	# 把第7行到10行替换
````

### 多窗口/多标签

```shell
:new filename		# 多窗口	ctrl+ww 切换光标
:vnew				# 垂直多窗口
#或者用`:sp`和`:vsp`

:tabnew				# 新标签页	gt：下一页	gT：上一页
:bd		# 关闭当前标签页
```

### 跳转

```bash
`显示行号`
:set nu

`字符串间跳转`
w：跳到串首
e：跳到串尾

n：匹配结果下跳
N：匹配结果上跳

`行间跳转`
gg			跳到第一行
G			跳到最后一行
[num]gg		跳到指定行
:[num] 		跳到指定行

```

### 删除

```bash
# 删除一个字符
x

# 删除单行
dd

# 删除到下一个？
dt

# 删除光标后多行
[num]dd

# 删除指定多行
:[start],[end]d	
# .：当前行
# $：最后一行
# %：所有行

# 删除所有行
%d

# 删除包含模式行
:g/[pattern]/d
:g!/[pattern]/d # 不匹配的模式行前添加!
# 例子：
:g/hahah/d # 删除所有包含“hahah”的行
:g/^#/d #删除以#开头的行
```

### 复制粘贴

```bash
# 选中
v+光标移动
V	# 按行选择

# 复制
y		# 复制选中
yy		# 复制当前行
yw		# 复制当前单词
y?bar	# 复制到上一个出现bar的位置

# 粘贴
P	# 粘贴到光标前
p	# 粘贴到光标后
```

### 文件对比

```shell
vimdiff file1 file2
```

## [apt](https://blog.csdn.net/digitalkee/article/details/104256472#:~:text=sudo%20apt-get%20remove%20--purge%20%E8%BD%AF%E4%BB%B6%E5%90%8D%20sudo%20apt-get%20autoremove,%E2%80%98%20%7Bprint%20%242%7D%E2%80%99%20%7Csudo%20xargs%20dpkg%20-P%20%E6%B8%85%E9%99%A4%E6%AE%8B%E4%BD%99%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E4%BF%9D%E8%AF%81%E5%B9%B2%E5%87%80)

```bash
sudo vi /etc/apt/source.list
# 阿里云镜像站
https://developer.aliyun.com/mirror/
# 清华镜像站
https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/
```

```bash
sudo apt-get update
#这个命令，会访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑。
我们在新立得软件包管理器里看到的软件列表，都是通过update命令更新的。
```

```bash
sudo apt-get upgrade
#这个命令，会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，
就会提示你更新。如果你的软件都是最新版本，会提示：
    升级了 0 个软件包，新安装了 0 个软件包，要卸载 0 个软件包，有 0 个软件包未被升级。
```

```bash
# 清理缓存
sudo apt-get autoclean # 清理旧版本的软件缓存
sudo apt-get clean # 清理所有软件缓存
sudo apt-get autoremove # 删除系统不再使用的孤立软件
```

### dpkg

> 包管理的临时文件目录:
> 包在：/var/cache/apt/archives
> 没有下载完的在：/var/cache/apt/archives/partial

### 删除软件

> ubuntu软件的删除一般用“ubuntu软件中心”或“新立得”就能搞定，但有时用命令似乎更快更好～～

```bash
sudo apt-get remove --purge 软件名
sudo apt-get autoremove # 删除系统不再使用的孤立软件
sudo apt-get autoclean # 清理旧版本的软件缓存
dpkg -l |grep ^rc|awk ‘{print $2}’ |sudo xargs dpkg -P # 清除残余的配置文件保证干净
```



## [aria2](https://www.escapelife.site/posts/7a3b6469.html)

下载：

```bash
apt install aria2
```

使用：

```bash
aria2c "download_site"
# -Z 下载多个文件
aria2c -Z "download_site_1" "download_site_2"
# -P 扩展下载地址
aria2c -P "download_site/image{1,2,3}_{A,B,C}.png"
# -o 指定保存文件名
aria2c -o index.html "download_site"
# -c 断点续传
aria2c -c "download_site"
# -d 指定文件夹
aria2c -d ./ "download_site"
# -x 分段下载
aria2c -x 8 "download_site"
# -s 下载最大并行进程数
aria2c -s 8 "download_site"
# -i 从文件获取下载地址
aria2c -i download.txt
# -S 列出磁力链地址的下载内容
aria2c -S "*.torrent"
# --select-file=<index> 指定索引下载磁力链内容
aria2c --select-file=1,3 "*.torrent"
```



## [bypy](https://github.com/houtianze/bypy)

下载：

```bash
pip install requests
pip install bypy
```

使用

```bash
# 获取百度云盘的授权
bypy info
# 云盘中生成一个bypy文件夹，把要下载的文件放入其中
# 指定路径上传
bypy upload [localpath] [remotepath]
# 下载单个文件
bypy downfile <remotefile> [localpath]
# 下载路径（文件夹）下载
bypy downdir [remotedir] [localdir]
# 显示进度
-v
```

## aliyunpan

下载：

[Releases · tickstep/aliyunpan (github.com)](https://github.com/tickstep/aliyunpan/releases)

使用：

```bash
# cd aliyunpan_dir
# 帮助
./aliyunpan help
# 登录
./aliyunpan login 
# 退出登录
./aliyunpan logout
# 切换网盘
./aliyunpan drive <driveId>
# 文件操作
./aliyunpan cd/ls/...
# 设置下载存储路径
./aliyunpan config set -savedir <savedir>
# 下载
./aliyunpan download|d <网盘文件或目录的路径1> <文件或目录2> ...
	--ow            overwrite, 覆盖已存在的文件
	--skip          skip same name, 跳过已存在的同名文件，即使文件内容不一致(不检查SHA1)
	--status        输出所有线程的工作状态
	--save          将下载的文件直接保存到当前工作目录
	--saveto value  将下载的文件直接保存到指定的目录
	-x              为文件加上执行权限, (windows系统无效)
	-p value        指定下载线程数 (default: 0)
	-l value        指定同时进行下载文件的数量 (default: 0)
	--retry value   下载失败最大重试次数 (default: 3)
	--nocheck       下载文件完成后不校验文件
	--exn value     指定排除的文件夹或者文件的名称，只支持正则表达式。支持排除多个名称，每一个名称就是一个exn参数
# 上传
./aliyunpan upload|u <本地文件/目录的路径1> <文件/目录2> ...  <目标目录>
```



## 删除大量文件

```bash
mkdir ddd
rsync -a --delete ddd/ test/
```



## 挂梯子

[让你的 Linux 云服务器也能科学上网！ · 无存在感小透明 (huaji.store)](https://ry.huaji.store/2020/08/Linux-magic-network/)

## ERR

[apt-get安装出现E: Sub-process /usr/bin/dpkg returned an error code (1)问题_apt error code 1-CSDN博客](https://blog.csdn.net/wanttifa/article/details/107548931)



