# 研发环境 ODV
## 1. 订购
用户登录联通云后 ，在顶部导航中找到【产品】栏，点击【网络数据平台】进入产品介绍页面：

 ![图片 5](/img/devvm/登1.png)
在产品介绍页面点击【立即选购】，跳转至网络数据平台页面：

注意：确保联通云用户处于登录状态，未登录无法访问网络数据平台页面。
 ![图片 研1](/img/devvm/研1.png)
 点击【研发环境】跳转至研发环境实例列表页面：
![图片 研登](/img/devvm/研登.png)
点击【新建】进入订购页面：
![图片 研2](/img/devvm/研2.png)

在订购页面中，按需自主订购研发环境产品，可自主选择所需CPU、内存（存储默认为30G系统盘+50G数据盘）：
 ![图片 研4](/img/devvm/研4.png)
## 2. 控制台
用户订购后，会自动跳转到产品的实例列表页面，可通过点击页面中的“刷新”按钮，获取最新状态的虚拟机状态。同时，可在实例列表页面对其进行启停、资源配额调整、退订等操作管理。

 ![图片 8](/img/devvm/4.png)

### 2.1启动/停止

状态为“运行中”的虚拟机，可通过操作栏中的“停止”操作，对虚拟机进行关机；

状态为“关机”的虚拟机，可通过操作栏中的“启动”操作，对虚拟机进行开机。

 ![图片 9](/img/devvm/5.png)

### 2.2资源调整

状态为“关机”的虚拟机，可通过操作栏中的“调整资源”操作，对虚拟机进行资源（CPU、内存）调整。

![图片 10](/img/devvm/6.png)

## 3. 产品使用

### 3.1初始化票据-上传/下载文件

当虚拟机的状态为“运行中”时，可通过操作栏中的“详情”操作，进入实例详情：

![图片 11](/img/devvm/9.png)

点击左侧【上传/下载文件】栏，找到/home/dev目录（**注意该页面上传文件的权限为dev用户，只有/home/dev目录有上传文件权限**），点击“上传”按钮，将用户的keytab文件（票据）上传到虚拟机：

![图片 12](/img/devvm/7.png)

![图片 13](/img/devvm/10.png)

### 3.2初始化票据-Terminal终端

上述票据上传完成后，进入Terminal终端进行票据解压：

 ![图片 14](/img/devvm/8.png)

进入“/home/dev”目录并解压票据zip包：

```
[dev@odatancmp-pxqbmmfr ~]$ sudo yum  install unzip -y 
... 
[dev@odatancmp-pxqbmmfr ~]$ cd /home/dev 
[dev@odatancmp-pxqbmmfr ~]$ ls  
code-server log.log   logs   zb_zhw_yw_odatatest_keytab.zip  
[dev@odatancmp-pxqbmmfr ~]$ unzip  zb_zhw_yw_odatatest_keytab.zip 
```

  初始化租户票据 ：

```
kinit zb_zhw_yw_odatatest@ODATA.NCMP.UNICOM.LOCAL -kt zb_zhw_yw_odatatest.keytab 
```

![图片 15](/img/devvm/11.png)

### 3.3 初始化环境

默认登陆自动初始化yarn2 

**初始化yarn1**

source /opt/beh/conf/yarn1/yarn1_env.sh

**初始化 yarn2**

source /opt/beh/conf/yarn2/yarn2_env.sh

**初始化 yarn3**

source /opt/beh/conf/yarn3/yarn3_env.sh

**初始化 yarn4**

source /opt/beh/conf/yarn4/yarn4_env.sh

以下说明以yarn2为例:

### 3.4 Hive使用-Terminal终端

（1）用户进行初始化票据之后，可通过beeline连接访问底座Hive集群：

 **//yarn2**

```
beeline -u 'jdbc:hive2://x86-gx-0008.odata.ncmp.unicom.local:2181,x86-gx-0009.odata.ncmp.unicom.local:2181,x86-gx-0010.odata.ncmp.unicom.local:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2'
```

 **注**：

  ①  **//yarn1** 

```
beeline -u  'jdbc:hive2://arm-gx-0008.odata.ncmp.unicom.local:2181,arm-gx-0009.odata.ncmp.unicom.local:2181,arm-gx-0010.odata.ncmp.unicom.local:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2'
```

 ②  **//yarn3** 

```
beeline -u  'jdbc:hive2://x86-gx-0664.odata.ncmp.unicom.local:2181,x86-gx-0665.odata.ncmp.unicom.local:2181,x86-gx-0666.odata.ncmp.unicom.local:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2' 
```

  ③   **//yarn4** 

```
beeline -u 'jdbc:hive2://x86-gx-1306.odata.ncmp.unicom.local:2181,x86-gx-1307.odata.ncmp.unicom.local:2181,x86-gx-1308.odata.ncmp.unicom.local:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2'  
```

 ![图片 16](/img/devvm/12.png)

(2)查询数据库列表

```
show databases;
```

 ![图片 17](/img/devvm/13.png)

(3)设置相应队列,执行相应语句.

### 3.5 Spark使用-Terminal终端

```
spark-submit --class org.apache.spark.examples.SparkPi --queue default  --master yarn  --deploy-mode cluster  --num-executors 10  /opt/beh/core/spark/examples/jars/spark-examples_2.12-3.1.1.jar  
```

![图片 18](/img/devvm/14.png)

本地调试demo：[任务模版](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/tree/master/spark%E7%A4%BA%E4%BE%8B/sparkodatatest)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）

如若您需要使用spark-shell命令进行调试，需要手动指定`--conf "spark.driver.bindAddress"='127.0.0.1'`，例如：
```
spark-shell --master local[*] --deploy-mode client --conf "spark.driver.bindAddress"='127.0.0.1'

```

### 3.6 Flink使用-Terminal终端

```
flink run -m yarn-cluster -yqu default -yD  security.kerberos.login.use-ticket-cache=true  /opt/beh/core/flink/examples/batch/WordCount.jar  
```

![图片 19](/img/devvm/15.png)

本地调试demo：[任务模版](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/tree/master/Flink%E7%A4%BA%E4%BE%8B/flinktest)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）

### 3.7 mr使用-Terminal终端

```
hadoop jar  $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.1.jar pi  -Dmapreduce.job.queuename=default 10 10 
```

 ![图片 20](/img/devvm/16.png)

### 3.8开发平台（vscode）

用户在左侧【概览信息】页面点击“开发平台”。（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**） ![图片 21](/img/devvm/17.png)

进入vscode页面：  ![图片 22](/img/devvm/18.png)

在vscode页面中，点击左侧上方栏目进入开发平台页面：

![图片 23](/img/devvm/19.png)

初始化环境(参考3.3章节)以及票据后，可正常访问集群，使用方式同上述各个组件方式（以mapreduce为例）：

![图片 24](/img/devvm/20.png)

### 3.9 home目录下创建一个和dev同级的目录

  1.首先利用python获取加密密码： 

```
[dev@wdhtest2-yoerls8l ~]$ python
Python 2.7.5 (default, Nov 16 2020, 22:23:17) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
\>>> import crypt
\>>> print crypt.crypt('test3','12')
12/8YkHioExfE
\>>>quit()
```

其中 12/8YkHioExfE 为 test3 加密后的密文 

2.新建用户，执行：  sudo useradd <用户> -p <加密后密码>  

3.删除误增的用户：  sudo userdel <用户>  

4.登陆用户：  su - <用户>  

5.上传文件到该用户目录下：  利用xshell或者其它软件，使用sftp协议，根据ssh信息连接上传文件即可。        
### 3.10 本地vscode免密配置

打开命令窗口cmd

执行ping 172.24.123.21,查看网络是否可达，网络没问题再往下执行

1、点击下载按钮，出现选项，根据自身系统选择需要下载的脚本，windows系统选择windos，mac系统选择mac。

![图片 29](/img/devvm/25.png)

2、下载完成之后解压run.zip 

![图片 30](/img/devvm/图片30.png)

3、（win系统）解压完成之后里面包含三个文件，首先看下Readme.txt看电脑是否满足运行脚本条件，如果满足，双击run.exe，运行时间较长，请耐心等待。

  （mac系统）解压完成后也是三个文件，和windows不通的是run代替了run.exe，打开终端terminal，进入run脚本所在目录，执行：./run就可以运行

![图片 31](/img/devvm/图片31.png)

![图片 32](/img/devvm/图片32.png)

执行脚本的注意事项：

脚本一定要执行完毕，脚本出现如下框内内容才能退出

![图片33](/img/devvm/图片33.png)

### 3.11 支持TFS Gitlab和Nexus

研发环境VM虚拟机默认maven配置，使用TFS的nexus库，同时支持连接TFS的Gitlab，进行效能平台代码文件的拉取。

![图片33](/img/devvm/maven.png)

进入TFS代码，右侧复制克隆地址，替换下方代理ip，执行git clone 克隆地址。

172.17.54.101:8080

![图片33](/img/devvm/gitlab.png)