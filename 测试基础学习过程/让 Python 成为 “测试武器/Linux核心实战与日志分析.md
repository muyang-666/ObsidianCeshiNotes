
### 📅 **第2天学习目标与能力图谱**

| **维度**   | **具体内容**                        |     |
| -------- | ------------------------------- | --- |
| **终极目标** | 掌握Linux高频命令组合，独立完成日志分析与测试环境部署   |     |
| **能力验证** | 能通过SSH连接服务器 → 定位接口报错 → 部署测试环境   |     |
| **秋招价值** | 面试必考Linux命令实操，日志分析能力=测试工程师核心竞争力 |     |


### ⚡ **第2天高效执行表（早中晚分段攻坚）**

|  **时间段**  |   **学习任务**   |                                                                         **详细执行步骤**                                                                          |          **产出物**          |                  **国内资源链接**                   | Yes/No |
| :-------: | :----------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------: | :-----------------------: | :-------------------------------------------: | ------ |
| **上午 3h** | Linux命令组合实战  |                           ==1. 安装WSL2：`wsl --install`（Win10/11）==  <br><br>2. 跟敲命令：`grep "ERROR" /mock/logs/app.log \| tail -n 5`                           |       Linux命令笔记.md        |      www.bilibili.com/video/BV1n84y1i7td      |        |
| **下午 3h** | Docker测试环境搭建 | 1. 安装[Docker Desktop中文版](https://www.docker.com/products/docker-desktop/) <br>2. 黑马B站教程部署MySQL+SpringBoot  <br>3. 制造接口错误：`curl http://localhost:8080/error` |  环境部署录屏.mp4  <br>错误日志截图   |                                               |        |
| **晚上 1h** |   日志分析报告制作   |                                               1. 用Excel统计日志错误类型分布  <br>2. Markdown编写分析结论<br><br>3. 上传GitHub仓库                                               | 日志分析报告.md  <br>GitHub提交记录 | [Typora中文版](https://typoraio.cn/)（Markdown工具） |        |

🌐 
### 🎯 **实操产出物**
1.**Linux命令速查手册**（Markdown格式）：
   ## 测试工程师高频命令
| 场景   | 命令组合                                    | 示例                      |
| ---- | --------------------------------------- | ----------------------- |
| 日志监控 | `tail -f logfile \| grep "ERROR"`       | 实时捕获支付接口错误              |
| 环境部署 | `docker run -d -p 8080:80 nginx:alpine` | 10秒搭建测试环境               |
| 权限修复 | `chmod +x test.sh && ./test.sh`         | 解决"Permission denied"问题 |
2.**日志分析报告**（截图+分析）：

- **故障现象**：用户注册接口500错误
    
- **定位过程**：
```bash
# 从10万行日志中快速定位
zgrep "register failed" api.log.20240531.gz | awk '{print $4}' | sort | uniq -c

```
- **根本原因**：空指针异常（NPE）
3.**Docker部署脚本**（GitHub托管）
```bash
#!/bin/bash
# 自动部署测试环境
docker-compose up -d mysql redis nginx
echo "测试环境已就绪！访问 http://localhost:8080"
```

## ***💡笔记***
#### **2.1 数据库和 SQL 概念**

**数据库（`Database`）是按照==数据结构来组织、存储和管理数据的仓库==，它的产生距今已有六十多年。随着信息技术和市场的发展，数据库变得无处不在：它在电子商务、银行系统等众多领域都被广泛使用，且成为其系统的重要组成部分。**

**数据库用于记录数据，使用数据库记录数据可以表现出各种数据间的联系，也可以很方便地对所记录的==数据进行增、删、改、查==等操作。**

**结构化查询语言(`Structured Query Language`)简称 SQL，是上世纪 70 年代由 IBM 公司开发，用于对数据库进行操作的语言。更详细地说，==SQL 是一种数据库查询和程序设计语言==，用于存取数据以及查询、更新和管理关系数据库系统，同时也是数据库脚本文件的扩展名。**

#### **2.2 MySQL 介绍**

**==MySQL 是一个 DBMS（数据库管理系统）==，由瑞典 ‘MySQLAB’公司开发，目前属于 Oracle 公司，MySQL 是最流行的关系型数据库管理系统（关系数据库，是建立在关系数据库模型基础上的数据库，借助于集合代数等概念和方法来处理数据库中的数据）。由于其体积小、速度快、总体拥有成本低，尤其是开放源码这一特点，一般中小型网站的开发者都选择 MySQL 作为网站数据库。==MySQL 使用 SQL 语言进行操作==。**

#### **3.1 安装之前的检查**
**![[Pasted image 20250708133734.png]]**      

#### **3.2 Ubuntu Linux 安装配置 MySQL**
**在 Ubuntu 上安装 MySQL，最简单的方式是在线安装。只需要几行简单的命令（ `#` 号后面是注释）**
```bash
#安装 MySQL 服务端、核心程序 
sudo apt-get install mysql-server 
#安装 MySQL 客户端 
sudo apt-get install mysql-client
```
**在安装过程中会提示确认输入 YES，设置 root 用户密码（之后也可以修改）等，稍等片刻便可安装成功。**

**安装结束后，用命令验证是否安装并启动成功：**

```bash
sudo netstat -tap | grep mysql
```

**如果出现如下提示，则安装成功：**

**![1-02](https://doc.shiyanlou.com/MySQL/sql-01-02.png)**

**此时，可以根据自己的需求，用 gedit 修改 MySQL 的配置文件（my.cnf）,使用以下命令:**

```bash
sudo gedit /etc/mysql/my.cnf
```

**至此，MySQL 已经安装、配置完成，可以正常使用了。**

#### **3.3 尝试 MySQL**
使用如下两条命令，打开 MySQL 服务并使用 root 用户登录：

```bash
# 启动 MySQL 服务
sudo service mysql start

# 使用 root 用户登录，实验楼环境的密码为空，直接回车就可以登录
mysql -u root
```

执行成功会出现如下提示：

![1-03](https://doc.shiyanlou.com/MySQL/sql-01-03-.png)

**2). 查看数据库：**

使用命令 `show databases;`，查看有哪些数据库（注意不要漏掉分号 `;`）：

![1-04](https://doc.shiyanlou.com/MySQL/sql-01-04.png)

可见已有三个数据库，分别是 “information-schema”、“mysql”、“performance-schema”。

**3). 连接数据库：**

选择连接其中一个数据库，语句格式为 `use <数据库名>`，这里可以不用加分号，这里我们选择 `information_schema` 数据库：

```sql
use information_schema_
```

![1-05](https://doc.shiyanlou.com/MySQL/sql-01-05.png)

**4). 查看表：**

使用命令 `show tables;` 查看数据库中有哪些表（**注意不要漏掉“;”**）：

![1-06](https://doc.shiyanlou.com/MySQL/sql-01-06.png)

**5). 退出：**

使用命令 `quit` 或者 `exit` 退出 MySQL。



#### **Linux 为何物**

  #### Linux 就是一个操作系统，就像你多少已经了解的 Windows（xp，7，8）和 Mac OS 。至于操作系统是什么，就不用过多解释了，如果你学习过前面的入门课程，应该会有个基本概念了，这里简单介绍一下操作系统在整个计算机系统中的角色。

![图1-1](https://doc.shiyanlou.com/linux_base/1-1.png)

### 我们的 Linux 主要是系统调用和内核那两层。当然直观地看，我们使用的操作系统还包含一些在其上运行的应用程序，比如文本编辑器、浏览器、电子邮件等。


### 1. **安装WSL2：`wsl --install`（Win10/11）**
  **控制面板->程序->程序与功能->启动或关闭windows功能**
![[Pasted image 20250708210453.png]]
**重启电脑后，打开windows商店，下载Ubuntu**


### **==Linux基础命令==
命令基础格式**
![[Pasted image 20250708222522.png]]、
==**命令： infonig可以查看IP地址**==

---

   ==**ls命令==**![Pasted image 20250708224441.png](<Pasted image 20250708224441.png>)
  

---

   **ls 命令的 -a选项
   **![Pasted image 20250708225222.png](<Pasted image 20250708225222.png>)
   

---

   **ls 命令的 -l选项**![Pasted image 20250708225440.png](<Pasted image 20250708225440.png>)
   可以相互组合如：ls -la /

---

**ls 命令的 -h选项
**![Pasted image 20250708225848.png](<Pasted image 20250708225848.png>)

---

==**cd   切换工作目录==
**![Pasted image 20250708230218.png](<Pasted image 20250708230218.png>)

---
 ==** *pwd* 查看当前工作目录**==
![Pasted image 20250708230533.png](<Pasted image 20250708230533.png>)

---

**相对路径与绝对路径**

![Pasted image 20250708231416.png](<Pasted image 20250708231416.png>)
![Pasted image 20250708231323.png](<Pasted image 20250708231323.png>)

---
==**mkdir命令**==

![Pasted image 20250708233433.png](<Pasted image 20250708233433.png>)
 
  **mkdir-p选项**![Pasted image 20250708234035.png](<Pasted image 20250708234035.png>)

---
**==touch   创建文件**==

![[Pasted image 20250709165134.png]]


---
**==cat命令  查看文件内容==**

![[Pasted image 20250709165412.png]]
**==more 命令查看文件内容==**
![[Pasted image 20250709165538.png]]

---
**==cp 命令复制文件文件夹==**

![[Pasted image 20250709165905.png]]


---
**==mv 移动文件文件夹==**![[Pasted image 20250709170310.png]]


---
**==rm 命令 删除文件/夹==**

![[Pasted image 20250709170807.png]]
**==rm 通配符==**
![[Pasted image 20250709171101.png]]


---
**==which 命令==**

![[Pasted image 20250709171533.png]]


---
**==find命令 按文件名查找文件（可以通配符查找）==**
![[Pasted image 20250709171850.png]]

  **==find 命令 按文件大小查找文件==**

![[Pasted image 20250709172339.png]]

---
**==grep 命令==**
![[Pasted image 20250709190630.png]]

---
**==wc 命令 做数据统计==**
![[Pasted image 20250709190946.png]]

---
**==管道符==**![[Pasted image 20250709191739.png]]

---
==**echon 命令**==

![[Pasted image 20250709192006.png]]

---
==**反引号**==

![[Pasted image 20250709192305.png]]

---
**==重定向符==**

![[Pasted image 20250709192532.png]]

---
**==tail 命令==**

![[Pasted image 20250709192743.png]]

---
**==vi/vim 的三种命令模式==**

![[Pasted image 20250709205148.png]]
==**命令模式的快捷键**==
![[Pasted image 20250709205030.png]]![[Pasted image 20250709205241.png]]
![[Pasted image 20250709205408.png]]

---

![[Pasted image 20250709230352.png]]
   **2.1查看进程信息**![[Pasted image 20250709231326.png]]
   ![[Pasted image 20250710001242.png]]

---
**端口号**![[Pasted image 20250710003026.png]]
![[Pasted image 20250710003327.png]]
![[Pasted image 20250710003417.png]]


### **常见的日志**
在 Linux 中大部分的发行版都内置使用 syslog 系统日志，那么通过前期的课程我们了解到常见的日志一般存放在 `/var/log` 中，我们来看看其中有哪些日志

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081469406921904)

根据图中所显示的日志，我们可以根据服务对象粗略的将日志分为两类

- 系统日志
- 应用日志

系统日志主要是存放系统内置程序或系统内核之类的日志信息如 `alternatives.log` 、`btmp` 等等，应用日志主要是我们装的第三方应用所产生的日志如 `tomcat7` 、`apache2` 等等。

接下来我们来看看常见的系统日志有哪些，他们都记录了怎样的信息

|日志名称|记录信息|
|---|---|
|alternatives.log|系统的一些更新替代信息记录|
|apport.log|应用程序崩溃信息记录|
|apt/history.log|使用 apt-get 安装卸载软件的信息记录|
|apt/term.log|使用 apt-get 时的具体操作，如 package 的下载、打开等|
|auth.log|登录认证的信息记录|
|boot.log|系统启动时的程序服务的日志信息|
|btmp|错误的信息记录|
|Consolekit/history|控制台的信息记录|
|dist-upgrade|dist-upgrade 这种更新方式的信息记录|
|dmesg|启动时，显示屏幕上内核缓冲信息，与硬件有关的信息|
|dpkg.log|dpkg 命令管理包的日志。|
|faillog|用户登录失败详细信息记录|
|fontconfig.log|与字体配置有关的信息记录|
|kern.log|内核产生的信息记录，在自己修改内核时有很大帮助|
|lastlog|用户的最近信息记录|
|wtmp|登录信息的记录。wtmp 可以找出谁正在进入系统，谁使用命令显示这个文件或信息等|
|syslog|系统信息记录|

而在本实验环境中没有 apport.log 是因为 apport 这个应用程序需要读取一些内核的信息来收集判断其他应用程序的信息，从而记录应用程序的崩溃信息。而在本实验环境中我们没有这个权限，所以将 apport 从内置应用值剔除，自然而然就没有它的日志信息了。

只闻其名，不见其人，我们并不能明白这些日志记录的内容。首先我们来看 `alternatives.log` 中的信息，在本实验环境中没有任何日志输出是因为刚刚启动的系统中并没有任何的更新迭代。我可以看看从其他地方截取过来的内容

```txt
update-alternatives 2016-07-02 13:36:16: run with --install /usr/bin/x-www-browser x-www-browser /usr/bin/google-chrome-stable 200
update-alternatives 2016-07-02 13:36:16: run with --install /usr/bin/gnome-www-browser gnome-www-browser /usr/bin/google-chrome-stable 200
update-alternatives 2016-07-02 13:36:16: run with --install /usr/bin/google-chrome google-chrome /usr/bin/google-chrome-stable 200
```

我们可以从中得到的信息有程序作用，日期，命令，成功与否的返回码。

我们用这样的命令来看看 `auth.log` 中的信息：

```bash
sudo less /var/log/auth.log
```

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081469409885670)

我们可以从中得到的信息有日期与 ip 地址的来源以及的用户与工具。

在 `/var/log/apt` 文件夹中有两个日志文件 `history.log` 与 `term.log`，两个日志文件的区别在于 `history.log` 主要记录了进行了哪个操作，相关的依赖有哪些，而 `term.log` 则是较为具体的一些操作，主要就是下载包，打开包，安装包等等的细节操作。

如果是刚刚开启的新系统，那么按理说这些日志应该都是空的。

```bash
sudo cat /var/log/apt/history.log
sudo cat /var/log/apt/term.log
```

![图片描述](https://doc.shiyanlou.com/courses/1379/871732/7bc699b28ff48cefee31fa859af518d9-0)

但是在实验环境中因为是启动的我们定制后的环境，所以两个日志中还残留了配置镜像的记录。可以先删除这两个文件然后再执行新的安装命令。

```bash
sudo rm /var/log/apt/history.log
sudo rm /var/log/apt/term.log
```

我们来安装 git 这个程序，因为实验环境里已经预装了 git，所以这里真正执行的操作是一个更新的操作，但这并不影响。

```bash
sudo apt-get install git
```

![图片描述](https://doc.shiyanlou.com/courses/uid871732-20200807-1596779861997)

成功的执行之后我们再来查看两个日志的内容变化：

![图片描述](https://doc.shiyanlou.com/courses/1379/871732/dc97b5ebc3c1054d4e5a6070fa36773b-0)

其他的日志格式也都类似于之前我们所查看的日志，主要便是时间，操作。而这其中有两个比较特殊的日志，其查看的方式比较与众不同，因为这两个日志并不是 ASCII 文件而是被编码成了二进制文件，所以我们并不能直接使用 less、cat、more 这样的工具来查看，这两个日志文件是 wtmp，lastlog

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081469412303087)

我们查看的方法是使用 last 与 lastlog 工具来提取其中的信息

![实验楼](https://dn-simplecloud.shiyanlou.com/1135081469412472830)

关于这两个工具的更深入使用我们可以使用前面的学习过的 man 来查看


