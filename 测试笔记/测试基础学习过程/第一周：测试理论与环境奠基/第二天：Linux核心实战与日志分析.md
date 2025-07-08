
### 📅 **第2天学习目标与能力图谱**

| **维度**   | **具体内容**                        |     |
| -------- | ------------------------------- | --- |
| **终极目标** | 掌握Linux高频命令组合，独立完成日志分析与测试环境部署   |     |
| **能力验证** | 能通过SSH连接服务器 → 定位接口报错 → 部署测试环境   |     |
| **秋招价值** | 面试必考Linux命令实操，日志分析能力=测试工程师核心竞争力 |     |


### ⚡ **第2天高效执行表（早中晚分段攻坚）**

| **时间段**   | **学习任务**     | **详细执行步骤**                                                                                                                                                           | **产出物**                   | **国内资源链接**                                                                         | Yes/No |
| --------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | ---------------------------------------------------------------------------------- | ------ |
| **上午 3h** | Linux命令组合实战  | ==1. 安装WSL2：`wsl --install`（Win10/11）==  <br>2. 完成实验楼[Linux命令基础课](https://www.lanqiao.cn/courses/1)第1-4关  <br>3. 跟敲命令：`grep "ERROR" /mock/logs/app.log \| tail -n 5` | Linux命令笔记.md              | [实验楼-Linux入门](https://www.lanqiao.cn/courses/1)www.bilibili.com/video/BV1n84y1i7td |        |
| **下午 3h** | Docker测试环境搭建 | 1. 安装[Docker Desktop中文版](https://www.docker.com/products/docker-desktop/)  <br>2. 黑马B站教程部署MySQL+SpringBoot  <br>3. 制造接口错误：`curl http://localhost:8080/error`         | 环境部署录屏.mp4  <br>错误日志截图    | [黑马Docker实战](https://www.bilibili.com/video/BV1S5411L7Fg)（P5-P7）                   |        |
| **晚上 1h** | 日志分析报告制作     | 1. 用Excel统计日志错误类型分布  <br>2. Markdown编写分析结论  <br>3. 上传GitHub仓库                                                                                                        | 日志分析报告.md  <br>GitHub提交记录 | [Typora中文版](https://typoraio.cn/)（Markdown工具）                                      |        |

### 🌐 **顶级学习资源推荐**

#### 国内资源（B站实战优先）

| **类型**   | **资源名称**                                                      | **重点章节**         |
| -------- | ------------------------------------------------------------- | ---------------- |
| Linux命令  | [黑马Linux实战4小时](https://www.bilibili.com/video/BV1dt411Y7mu)   | P5-P8（日志分析）      |
| Docker部署 | [千锋Docker测试环境搭建](https://www.bilibili.com/video/BV1S5411L7Fg) | 实战部署SpringBoot应用 |
| 生产故障排查   | [腾讯课堂-线上BUG定位实战](https://ke.qq.com/course/687382)             | 模块二：日志分析技巧       |
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



### **Linux 为何物**

Linux 就是一个操作系统，就像你多少已经了解的 Windows（xp，7，8）和 Mac OS 。至于操作系统是什么，就不用过多解释了，如果你学习过前面的入门课程，应该会有个基本概念了，这里简单介绍一下操作系统在整个计算机系统中的角色。

![图1-1](https://doc.shiyanlou.com/linux_base/1-1.png)

我们的 Linux 主要是系统调用和内核那两层。当然直观地看，我们使用的操作系统还包含一些在其上运行的应用程序，比如文本编辑器、浏览器、电子邮件等。


### 1. **安装WSL2：`wsl --install`（Win10/11）**
  **控制面板->程序->程序与功能->启动或关闭windows功能**
![[Pasted image 20250708210453.png]]
**重启电脑后，打开windows商店，下载Ubuntu**


### **==Linux基础命令==
命令基础格式**
![[Pasted image 20250708222522.png]]、
**命令： infonig可以查看IP地址**

---

   ==**ls命令==**![[Pasted image 20250708224441.png]]
  

---

   **ls 命令的 -a选项
   **![[Pasted image 20250708225222.png]]
   

---

   **ls 命令的 -l选项**![[Pasted image 20250708225440.png]]
   可以相互组合如：ls -la /

---

**ls 命令的 -h选项
**![[Pasted image 20250708225848.png]]

---

==**cd切换工作目录==
**![[Pasted image 20250708230218.png]]

---
 ==** *pwd* 查看当前工作目录**==
![[Pasted image 20250708230533.png]]

---

**相对路径与绝对路径**

![[Pasted image 20250708231416.png]]
![[Pasted image 20250708231323.png]]

---
==**mkdir命令**==

![[Pasted image 20250708233433.png]]
 
  **mkdir-p选项**![[Pasted image 20250708234035.png]]

---

