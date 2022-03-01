# 任务调度 OTS
## 1. 概述
任务调度 OTS是基于Apache DolphinScheduler 的一个分布式去中心化，易扩展的可视化 DAG 工作流任务调度系统。
致力于解决数据处理流程中错综复杂的依赖关系，使调度系统在数据处理流程中开箱即用。《大数据统一底座-DolphinScheduler 操作说明书》是对 DolphinScheduler 软件的功能操作说明，指导用户正确使用该服务进行业务集成与操作。
## 2. 登录
● 登录联通云访问：联通云用户申请网络数据平台账号后，点击顶部产品列表中的【任务调度 Dolphinscheduler-数据域】跳转 DolphinScheduler。
![](/img/task/DS图片/1.png)
## 3.首页
● 首页包含用户所有项目的任务状态统计、流程状态统计、工作流定义统计：

![图片1](/img/task/DS图片/2.png)

## 4. 资源中心
### 4.1 文件管理

● 针对各种资源文件的管理，包括创建基本的 txt/log/sh/conf/py/java 等文件、上传 jar 包等各种类型文件，可进行编辑、重命名、下载、删除等操作。

![](/img/task/DS图片/图片2.png)

● 创建文件

![](/img/task/DS图片/图片3.png)

● 文件格式支持以下几种类型：txt、log、sh、conf、cfg、py、java、sql、xml、hql、properties

![](/img/task/DS图片/图片4.png)

● 上传文件：点击"上传文件"按钮进行上传，将文件拖拽到上传区域，文件名会自动以上传的文件名称补全

用户上传文件前需要先创建自己的文件夹，再在该文件夹下上传所需文件。

![](/img/task/DS图片/图片5.png)

● 文件查看

对可查看的文件类型，点击文件名称，可查看文件详情。

![](/img/task/DS图片/图片6.png)

● 下载文件

点击文件列表的"下载"按钮下载文件或者在文件详情中点击右上角"下载"按钮下载文件。

● 文件重命名

![](/img/task/DS图片/图片7.png)

● 删除

文件列表->点击"删除"按钮，删除指定文件。

![](/img/task/DS图片/图片8.png)

### 4.2UDF 管理

#### 4.2.1 资源管理

● 资源管理和文件管理功能类似，不同之处是资源管理是上传的 UDF 函数。

● 文件管理上传的是用户程序，脚本及配置文件 操作功能：重命名、下载、删除。

● 上传 udf 资源和上传文件相同。

#### 4.2.2 函数管理

● 创建 udf 函数。

● 点击“创建 UDF 函数”，输入 udf 函数参数，选择 udf 资源，点击“提交”，创建 udf 函数。

● 目前只支持 HIVE 的临时 UDF 函数。

● UDF 函数名称：输入 UDF 函数时的名称。

● 包名类名：输入 UDF 函数的全路径。

● UDF 资源：设置创建的 UDF 对应的资源文件。

![](/img/task/DS图片/8.png)

### 4.3 OPEN-API 使用示例/DS 资源中心上传脚本示例

通过 Dolphinscheduler 系统的 open-api 与第三方系统集成通过调用 API 来管理项目、流程。

通过 shell 脚本完成在资源中心的文件上传功能。

#### 4.3.1 使用 Token 调用 open-api

**打开 API 文档页面**

● HTML ⽂档地址：https://odata-task.console.tg.ncmp.unicom.local/dolphinscheduler/doc.html

![1](/img/task/Open-API图片/1.png)

● SWAGGER ⽂档地址：https://odata-task.console.tg.ncmp.unicom.local/dolphinscheduler/swagger-ui.html

![2](/img/task/Open-API图片/2.png)

#### 4.3.2 接口测试

##### 4.3.2.1Postman 接口测试

注意：实际在代码中的接口测试，请将域名替换为`172.24.123.21:12345`（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后测试**）

● 选⼀个测试的接⼝，本次测试选取的接⼝是：查询所有项⽬ /dolphinscheduler/projects/query-project-list

![3](/img/task/Open-API图片/3.png)

● 打开 Postman，填写接⼝地址，并在 Headers 中填写 Token，发送请求后即可查看结果

```
token:刚刚⽣成的Token
```

![5](/img/task/Open-API图片/5.png)

![6](/img/task/Open-API图片/6.png)

##### 4.3.2.2 Shell 接口测试

● 选择启动流程实例接⼝

![7](/img/task/Open-API图片/7.png)

● CURL 请求

```
curl -X POST -S \

http://172.24.123.21:12345/dolphinscheduler/projects/yuanqiang-project/executors/start-process-instance \
-H "token: 25c158c4107ea1f0730b1b90015a347c" \
-F "processDefinitionId=1" \
-F "scheduleTime=" \
-F "failureStrategy=CONTINUE" \
-F "warningType=NONE" \
-F "warningGroupId=0" \
-F "execType=" \
-F "startNodeList=" \
-F "taskDependType=TASK_POST" \
-F "runMode=RUN_MODE_SERIAL" \
-F "processInstancePriority=MEDIUM" \
-F "receivers=" \
-F "receiversCc=" \
-F "workerGroup=default"
```

● 正确返回

```
{"code":0,"msg":"success","data":null}
```

#### 4.3.3 资源中心上传文件脚本示例

根目录的 ID 为-1

##### 4.3.3.1 上传文件

●TOKEN 令牌：2f49f9726fe1105598216410ab0f5f82（示例）

● 上传⽂件：/home/yuan/kafka_client_jaas.conf

● ⽂件名称：kafka_client_jaas.conf

●DS 上⽬录：/hadoop (⽬录 ID 为 1)

shell 如下：

```
curl -X POST -sSL \
http://172.24.123.21:12345/dolphinscheduler/resources/create \
-H "Content-Type: multipart/form-data" \
-H "token: 2f49f9726fe1105598216410ab0f5f82" \
-H "Accept: application/json, text/plain, */*" \
-H "Connection: keep-alive" \
-H "Accept-Encoding: gzip, deflate" \
-F "file=@/home/yuan/kafka_client_jaas.conf" \
-F "type=FILE" \
-F "name=kafka_client_jaas.conf" \
-F "pid=1" \
-F "currentDir=/hadoop" \
-F "description="
```

##### 4.3.3.2 创建目录

●TOKEN 令牌：2f49f9726fe1105598216410ab0f5f82（示例）

● 创建⽬录：new_dir

● ⽗级⽬录：/hadoop (⽬录 ID 为 1)

shell 如下：

```
curl -X POST -sSL \
http://172.24.123.21:12345/dolphinscheduler/resources/directory/create \
-H "token: 2f49f9726fe1105598216410ab0f5f82" \
-F "type=FILE" \
-F "name=new_dir" \
-F "currentDir=/hadoop" \
-F "pid=1" \
-F "description="
```

## 5.数据源中心

### 5.1 创建数据源

● 数据源中心支持 MySQL、POSTGRESQL、HIVE/IMPALA、SPARK、CLICKHOUSE、ORACLE、SQLSERVER 等数据源。

#### 5.1.1 创建/编辑 MySQL 数据源

点击“数据源中心->创建数据源”，根据需求创建不同类型的数据源。

● 数据源：选择 MYSQL。

● 数据源名称：输入数据源的名称。

● 描述：输入数据源的描述。

● IP 主机名：输入连接 MySQL 的 IP。

● 端口：输入连接 MySQL 的端口。

● 用户名：设置连接 MySQL 的用户名。

● 密码：设置连接 MySQL 的密码。

● 数据库名：输入连接 MySQL 的数据库名称。

● Jdbc 连接参数：用于 MySQL 连接的参数设置，以 JSON 形式填写。

![](/img/task/DS图片/mysql数据源.png)

点击“测试连接”，测试数据源是否可以连接成功。

#### 5.1.2 创建/编辑 POSTGRESQL 数据源

● 数据源：选择 POSTGRESQL。

● 数据源名称：输入数据源的名称。

● 描述：输入数据源的描述。

● IP/主机名：输入连接 POSTGRESQL 的 IP。

● 端口：输入连接 POSTGRESQL 的端口。

● 用户名：设置连接 POSTGRESQL 的用户名。

● 密码：设置连接 POSTGRESQL 的密码。

● 数据库名：输入连接 POSTGRESQL 的数据库名称。

● Jdbc 连接参数：用于 POSTGRESQL 连接的参数设置，以 JSON 形式填写。

![](/img/task/DS图片/POSTGRESQL数据源.png)

#### 5.1.3 创建/编辑 HIVE 数据源

● 使用 zookeeper 方式连接。

● 下图中红框内信息为集群信息，租户用户使用时按照使用集群进行填写。
![](/img/task/DS图片/图片12.png)

● 其中

**YARN1 对应填写信息：**

IP 主机名：arm-gx-0008.odata.ncmp.unicom.local,arm-gx-0009.odata.ncmp.unicom.local,arm-gx-0010.odata.ncmp.unicom.local

端口：2181

hs2-principal：hadoop/_HOST@ODATA.NCMP.UNICOM.LOCAL

**YARN2 对应填写信息：**

IP 主机名：x86-gx-0008.odata.ncmp.unicom.local,x86-gx-0009.odata.ncmp.unicom.local,x86-gx-0010.odata.ncmp.unicom.local

端口：2181

hs2-principal：hadoop/_HOST@ODATA.NCMP.UNICOM.LOCAL

**YARN3 对应填写信息：**

IP 主机名：x86-gx-0664.odata.ncmp.unicom.local,x86-gx-0665.odata.ncmp.unicom.local,x86-gx-0666.odata.ncmp.unicom.local

端口：2181

hs2-principal：hadoop/_HOST@ODATA.NCMP.UNICOM.LOCAL

**YARN4 对应填写信息：**

IP 主机名：x86-gx-1306.odata.ncmp.unicom.local,x86-gx-1307.odata.ncmp.unicom.local,x86-gx-1308.odata.ncmp.unicom.local

端口：2181

hs2-principal：hadoop/_HOST@ODATA.NCMP.UNICOM.LOCAL

● 数据源：选择 HIVE。

● 数据源名称：输入数据源的名称 z。

● 描述：输入数据源的描述。

● IP/主机名：输入连接 HIVE 的 IP（按照 hive 所在集群填写）。

● 端口：输入连接 HIVE 的端口（无需变动）。

● Hs2-principle：服务端票据（无需变动）。

● 用户名：设置连接 HIVE 的用户名。

● 密码：设置连接 HIVE 的密码。

● 数据库名：输入连接 HIVE 的数据库名称。

● Jdbc 连接参数：用于 HIVE 连接的参数设置，以 JSON 形式填写。

#### 5.1.4 创建/编辑 Spark 数据源**（**暂不支持 zookeeper 高可用连接）

![](/img/task/DS图片/图片13.png)

● 数据源：选择 Spark。

● 数据源名称：输入数据源的名称。

● 描述：输入数据源的描述。

● IP/主机名：输入连接 Spark 的 IP。

● 端口：输入连接 Spark 的端口。

● 用户名：设置连接 Spark 的用户名。

● 密码：设置连接 Spark 的密码。

● 数据库名：输入连接 Spark 的数据库名称。

● Jdbc 连接参数：用于 Spark 连接的参数设置，以 JSON 形式填写。

注意：如果开启了 kerberos，则需要填写 Principal

![](/img/task/DS图片/图片14.png)

## 6.项目管理

### 6.1 创建项目

● 点击"项目管理"进入项目管理页面，点击“创建项目”按钮，输入项目名称，项目描述，点击“提交”，创建新的项目。

![](/img/task/DS图片/图片15.png)

### 6.1.1 项目首页

● 在项目管理页面点击项目名称链接，进入项目首页，如下图所示，项目首页包含该项目的任务状态统计、流程状态统计、工作流定义统计。

![](/img/task/DS图片/3.png)

● 任务状态统计：在指定时间范围内，统计任务实例中状态为提交成功、正在运行、准备暂停、暂停、准备停止、停止、失败、成功、需要容错、kill、等待线程的个数。

● 流程状态统计：在指定时间范围内，统计工作流实例中状态为提交成功、正在运行、准备暂停、暂停、准备停止、停止、失败、成功、需要容错、kill、等待线程的个数。

● 工作流定义统计：统计用户创建的工作流定义及管理员授予该用户的工作流定义。

#### 6.1.2 工作流定义

##### 6.1.2.1 创建工作流定义

● 点击项目管理->工作流->工作流定义，进入工作流定义页面，点击“创建工作流”按钮，进入工作流 DAG 编辑页面，如下图所示：

![](/img/task/DS图片/图片17.png)

● 工具栏中拖拽到画板中，新增一个 Shell 任务,如下图所示：

![](/img/task/DS图片/图片18.png)

添加 shell 任务的参数设置：

● 填写“节点名称”，“描述”，“脚本”字段；

“运行标志”勾选“正常”，若勾选“禁止执行”，运行工作流不会执行该任务；

● 选择“任务优先级”：当 worker 线程数不足时，级别高的任务在执行队列中会优先执行，相同优先级的任务按照先进先出的顺序执行。

注意：如果当前用户创建在 Yarn2 队列上，则 worker 分区选择 yarn2。用户创建在 Yarn1 队列上，这里则选择 yarn1。请正确选择 worker 分区，避免任务运行出错。

● 超时告警（非必选）：勾选超时告警、超时失败，填写“超时时长”，当任务执行时间超过超时时长，会发送告警邮件并且任务超时失败；

● 资源（非必选）。资源文件是资源中心->文件管理页面创建或上传的文件，如文件名为 test.sh，脚本中调用资源命令为 sh test.sh；

● 自定义参数（非必填），参考自定义参数；

点击"确认添加"按钮，保存任务设置。

● 增加任务执行的先后顺序：点击右上角图标连接任务；如下图所示，任务 2 和任务 3 并行执行，当任务 1 执行完，任务 2、3 会同时执行。

![](/img/task/DS图片/7.png)

● 删除依赖关系： 点击右上角"箭头"图标，选中连接线，点击右上角"删除"图标，删除任务间的依赖关系。

![](/img/task/DS图片/删除依赖关系.png)

● 保存工作流定义： 点击”保存“按钮，弹出"设置 DAG 图名称"弹框，如下图所示，输入工作流定义名称，工作流定义描述，设置全局参数（选填，参考自定义参数），点击"添加"按钮，工作流定义创建成功。

![](/img/task/DS图片/图片21.png)

##### 6.1.2.2 工作流定义操作功能

● 点击项目管理->工作流->工作流定义，进入工作流定义页面，如下图所示:

![](/img/task/DS图片/图片22.png)

工作流定义列表的操作功能如下：

● 编辑： 只能编辑"下线"的工作流定义。工作流 DAG 编辑同创建工作流定义。

● 上线： 工作流状态为"下线"时，上线工作流，只有"上线"状态的工作流能运行，但不能编辑。

● 下线： 工作流状态为"上线"时，下线工作流，下线状态的工作流可以编辑，但不能运行。

● 运行： 只有上线的工作流能运行。运行操作步骤见 2.3.3 运行工作流。

● 定时： 只有上线的工作流能设置定时，系统自动定时调度工作流运行。创建定时后的状态为"下线"，需在定时管理页面上线定时才生效。定时操作步骤见 2.3.4 工作流定时。

● 定时管理： 定时管理页面可编辑、上线/下线、删除定时。

● 删除： 删除工作流定义。

● 下载： 下载工作流定义到本地。

● 树形图： 以树形结构展示任务节点的类型及任务状态，如下图所示：

![](/img/task/DS图片/图片23.png)

##### 6.1.2.3 运行工作流

● 点击项目管理->工作流->工作流定义，进入工作流定义页面，如下图所示，点击"上线"按钮，上线工作流。

![](/img/task/DS图片/图片24.png)

● 点击”运行“按钮，弹出启动参数设置弹框，如下图所示，设置启动参数，点击弹框中的"运行"按钮，工作流开始运行，工作流实例页面生成一条工作流实例。

![](/img/task/DS图片/图片25.png)

工作流运行参数说明：

● 失败策略：当某一个任务节点执行失败时，其他并行的任务节点需要执行的策略。”继续“表示：某一任务失败后，其他任务节点正常执行；”结束“表示：终止所有正在执行的任务，并终止整个流程。

● 通知策略：当流程结束，根据流程状态发送流程执行信息通知邮件，包含任何状态都不发，成功发，失败发，成功或失败都发。

● 流程优先级：流程运行的优先级，分五个等级：最高（HIGHEST），高(HIGH),中（MEDIUM）,低（LOW），最低（LOWEST）。当 master 线程数不足时，级别高的流程在执行队列中会优先执行，相同优先级的流程按照先进先出的顺序执行。

●worker 分组：该流程只能在指定的 worker 机器组里执行。请选择与当前用户创建在同一 yarn 队列的 worker 分区，避免任务运行出错。

● 通知组：选择通知策略||超时报警||发生容错时，会发送流程信息或邮件到通知组里的所有成员。

● 收件人：选择通知策略||超时报警||发生容错时，会发送流程信息或告警邮件到收件人列表。


● 启动参数: 在启动新的流程实例时，设置或覆盖全局参数的值。

● 补数：包括串行补数、并行补数 2 种模式。串行补数：指定时间范围内，从开始日期至结束日期依次执行补数，依次生成 N 条流程实例；并行补数：指定时间范围内，多天同时进行补数，同时生成 N 条流程实例。

● 补数： 执行指定日期的工作流定义，可以选择补数时间范围（目前只支持针对连续的天进行补数)，比如需要补 5 月 1 号到 5 月 10 号的数据，如下图所示：

![](/img/task/DS图片/图片26.png)

● 串行模式：补数从 5 月 1 号到 5 月 10 号依次执行，依次在流程实例页面生成十条流程实例；

● 并行模式：同时执行 5 月 1 号到 5 月 10 号的任务，同时在流程实例页面生成十条流程实例。

##### 6.1.2.4 工作流定时

● 创建定时：点击项目管理->工作流->工作流定义，进入工作流定义页面，上线工作流，点击"定时"按钮,弹出定时参数设置弹框，如下图所示：

![](/img/task/DS图片/图片27.png)

● 选择起止时间。在起止时间范围内，定时运行工作流；不在起止时间范围内，不再产生定时工作流实例。

● 添加一个每天凌晨 5 点执行一次的定时，如下图所示：

![](/img/task/DS图片/图片28.png)

● 失败策略、通知策略、流程优先级、Worker 分组、通知组、收件人、抄送人同工作流运行参数。

点击"创建"按钮，创建定时成功，此时定时状态为"下线"，定时需上线才生效。

● 定时上线：点击"定时管理"按钮，进入定时管理页面，点击"上线"按钮，定时状态变为"上线"，如下图所示，工作流定时生效。

![](/img/task/DS图片/定时管理.png)

##### 6.1.2.5 导入工作流

● 点击项目管理->工作流->工作流定义，进入工作流定义页面，点击"导入工作流"按钮，导入本地工作流文件，工作流定义列表显示导入的工作流，状态为下线。

#### 6.1.3 工作流实例

##### 6.1.3.1 查看工作流实例

● 点击项目管理->工作流->工作流实例，进入工作流实例页面，如下图所示：

![](/img/task/DS图片/图片30.png)

● 点击工作流名称，进入 DAG 查看页面，查看任务执行状态，如下图所示。

![](/img/task/DS图片/图片31.png)

##### 6.1.3.2 查看任务日志

● 进入工作流实例页面，点击工作流名称，进入 DAG 查看页面，双击任务节点，如下图所示：

![](/img/task/DS图片/图片32.png)

● 点击"查看日志"，弹出日志弹框，如下图所示,任务实例页面也可查看任务日志，参考任务查看日志。

![](/img/task/DS图片/4.png)

##### 6.1.3.3 查看任务历史记录

● 点击项目管理->工作流->工作流实例，进入工作流实例页面，点击工作流名称，进入工作流 DAG 页面;

● 双击任务节点，如下图所示，点击"查看历史"，跳转到任务实例页面，并展示该工作流实例运行的任务实例列表。

![](/img/task/DS图片/图片34.png)

##### 6.1.3.4 查看运行参数

● 点击项目管理->工作流->工作流实例，进入工作流实例页面，点击工作流名称，进入工作流 DAG 页面;

● 点击左上角图标，查看工作流实例的启动参数；点击图标，查看工作流实例的全局参数和局部参数，如下图所示：

![](/img/task/DS图片/图片35.png)

##### 6.1.3.5 工作流实例操作功能

● 点击项目管理->工作流->工作流实例，进入工作流实例页面，如下图所示：

![](/img/task/DS图片/图片36.png)

● 编辑： 只能编辑已终止的流程。点击"编辑"按钮或工作流实例名称进入 DAG 编辑页面，编辑后点击"保存"按钮，弹出保存 DAG 弹框，如下图所示，在弹框中勾选"是否更新到工作流定义"，保存后则更新工作流定义；若不勾选，则不更新工作流定义。

![](/img/task/DS图片/6.png)

● 重跑： 重新执行已经终止的流程。

● 恢复失败： 针对失败的流程，可以执行恢复失败操作，从失败的节点开始执行。

● 停止： 对正在运行的流程进行停止操作，后台会先 killworker 进程,再执行 kill -9 操作。

● 暂停： 对正在运行的流程进行暂停操作，系统状态变为等待执行，会等待正在执行的任务

● 结束，暂停下一个要执行的任务。

● 恢复暂停： 对暂停的流程恢复，直接从暂停的节点开始运行。

● 删除： 删除工作流实例及工作流实例下的任务实例。

● 甘特图： Gantt 图纵轴是某个工作流实例下的任务实例的拓扑排序，横轴是任务实例的运行时间,如图示：

![](/img/task/DS图片/9.png)

#### 6.1.4 任务实例

● 点击项目管理->工作流->任务实例，进入任务实例页面，如下图所示，点击工作流实例名称，可跳转到工作流实例 DAG 图查看任务状态。

![](/img/task/DS图片/图片39.png)

● 查看日志：点击操作列中的“查看日志”按钮，可以查看任务执行的日志情况。

![](/img/task/DS图片/5.png)

## 7.监控中心

### 7.1 服务管理

● 服务管理主要是对系统中的各个服务的健康状况和基本信息的监控和显示。

**master 监控**

● 主要是 master 的相关信息。

![](/img/task/DS图片/图片41.png)

**worker 监控**

● 主要是 worker 的相关信息。

![](/img/task/DS图片/图片42.png)

**Zookeeper 监控**

● 主要是 zookpeeper 中各个 worker 和 master 的相关配置信息。

![](/img/task/DS图片/图片43.png)

**DB 监控**

● 主要是 DB 的健康状况。

![](/img/task/DS图片/图片44.png)

### 7.2 统计管理

![](/img/task/DS图片/图片45.png)

● 待执行命令数：统计 t_ds_command 表的数据。

● 执行失败的命令数：统计 t_ds_error_command 表的数据。

● 待运行任务数：统计 Zookeeper 中 task_queue 的数据。

● 待杀死任务数：统计 Zookeeper 中 task_kill 的数据。

## 8.任务节点类型和参数设置

### 8.1 Shell 节点

●shell 节点，在 worker 执行的时候，会生成一个临时 shell 脚本，使用租户同名的 linux 用户执行这个脚本。

● 点击项目管理-项目名称-工作流定义，点击"创建工作流"按钮，进入 DAG 编辑页面。

● 工具栏中拖动到画板中，如下图所示：

![](/img/task/DS图片/图片46.png)

● 节点名称：一个工作流定义中的节点名称是唯一的。

● 运行标志：标识这个节点是否能正常调度,如果不需要执行，可以打开禁止执行开关。

● 描述信息：描述该节点的功能。

● 任务优先级：优先级分五类从高到低，当 worker 线程数不足时，根据优先级从高到低依次执行，优先级一样时根据先进先出原则执行。

![](/img/task/DS图片/图片47.png)

● Worker 分组：任务分配给 worker 组的机器机执行，分组时要尤为注意选择需要执行的机器，如果默认选择 Default 则会随机选择一台 worker 机执行。

![](/img/task/DS图片/图片48.png)

● 失败重试次数：任务失败重新提交的次数，支持下拉和手填。

![](/img/task/DS图片/图片49.png)

● 失败重试间隔：任务失败重新提交任务的时间间隔，支持下拉和手填。

![](/img/task/DS图片/图片50.png)

● 超时告警：选择性勾选，若勾选超时告警或超时失败时，当任务超过"超时时长"后，会发送告警邮件并且任务执行失败 。

![](/img/task/DS图片/图片51.png)

● 脚本：用户开发的 SHELL 程序。

● 资源：是指脚本中需要调用的资源文件列表，资源中心-文件管理上传或创建的文件。

● 自定义参数：是 SHELL 局部的用户自定义参数，会替换脚本中以${变量}的内容。

### 8.2 SQL 节点

● 拖动工具栏中的任务节点到画板中。

● 数据源：选择对应的数据源。

![](/img/task/DS图片/图片52.png)

● sql 类型：支持查询和非查询两种。查询是 select 类型的查询，是有结果集返回的，可以指定邮件通知为表格、附件或表格附件三种模板；非查询是没有结果集返回的，是针对 update、delete、insert 三种类型的操作。

![](/img/task/DS图片/图片53.png)

![](/img/task/DS图片/图片54.png)

● sql 参数：输入参数格式为 key1=value1;key2=value2…

● sql 语句：SQL 语句。

● UDF 函数：对于 HIVE 类型的数据源，可以引用资源中心中创建的 UDF 函数,其他类型的数据源暂不支持 UDF 函数。

● 自定义参数：SQL 任务类型，而存储过程是自定义参数顺序的给方法设置值自定义参数类型和数据类型同存储过程任务类型一样。区别在于 SQL 任务类型自定义参数会替换 sql 语句中${变量}。

● 前置 sql:前置 sql 在 sql 语句之前执行。

● 后置 sql:后置 sql 在 sql 语句之后执行。

### 8.3 SPARK 节点

● 通过 SPARK 节点，可以直接直接执行 SPARK 程序，对于 spark 节点，worker 会使用 spark-submit 方式提交任务。

● 拖动工具栏中的任务节点到画板中：

● 程序类型：支持 JAVA、Scala 和 Python 三种语言。

![](/img/task/DS图片/图片55.png)

● 主函数的 class：是 Spark 程序的入口 Main Class 的全路径。

● 主 jar 包：是 Spark 的 jar 包。

● 部署方式：支持 yarn-cluster、yarn-client 和 local 三种模式。

● Driver 内核数：可以设置 Driver 内核数及内存数。

● Executor 数量：可以设置 Executor 数量、Executor 内存数和 Executor 内核数。

● 命令行参数(主程序参数)：是设置 Spark 程序的输入参数，支持自定义参数变量的替换。

● 其他参数(选项参数)：支持 --jars、--files、--archives、--conf 格式。

● 资源：如果其他参数中引用了资源文件，需要在资源中选择指定。

● 自定义参数：是 MR 局部的用户自定义参数，会替换脚本中以${变量}的内容。

注意：JAVA 和 Scala 只是用来标识，没有区别，如果是 Python 开发的 Spark 则没有主函数的 class，其他都是一样。

**Spark 节点案例：**

![](/img/task/DS图片/图片56.png)

### 8.4 MapReduce(MR)节点

● 使用 MR 节点，可以直接执行 MR 程序。对于 mr 节点，worker 会使用 hadoop jar 方式提交任务。

● 拖动工具栏中的任务节点到画板中：

![](/img/task/DS图片/图片57.png)

● 程序类型：分为 Java 程序和 Python 程序。

**当为 JAVA 程序时：**

![](/img/task/DS图片/图片58.png)

​

● 主函数的 class：是 MR 程序的入口 Main Class 的全路径。

● 主 jar 包：是 MR 的 jar 包。

● 命令行参数：是设置 MR 程序的输入参数，支持自定义参数变量的替换。

● 其他参数：支持 –D、-files、-libjars、-archives 格式。

● 资源： 如果其他参数中引用了资源文件，需要在资源中选择指定。

● 自定义参数：是 MR 局部的用户自定义参数，会替换脚本中以${变量}的内容。

**当为 Python 程序时**

![](/img/task/DS图片/图片59.png)

​

● 程序类型：选择 Python 语言。

● 主 jar 包：是运行 MR 的 Python jar 包。

● 其他参数：支持 –D、-mapper、-reducer、-input -output 格式，这里可以设置用户自定义参数的输入，比如：

-mapper "mapper.py 1" -file mapper.py -reducer reducer.py -file reducer.py –input /journey/words.txt -output /journey/out/mr/${currentTimeMillis}

● 其中 -mapper 后的 mapper.py 1 是两个参数，第一个参数是 mapper.py，第二个参数是 1。

● 资源： 如果其他参数中引用了资源文件，需要在资源中选择指定。

● 自定义参数：是 MR 局部的用户自定义参数，会替换脚本中以${变量}的内容。

**MR 节点案例：**

![](/img/task/DS图片/新1.jpg)

注意： 在写MR程序是要考虑到把票据作为参数传入。

### 8.5 Flink 节点

● 拖动工具栏中的任务节点到画板中，如下图所示：

![](/img/task/DS图片/flink2.png)

![](/img/task/DS图片/flink1.png)

上述flink节点配置程序demo：[任务模版](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/tree/master/Flink%E7%A4%BA%E4%BE%8B/flink-wordcount)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）

**说明：页面配置主程序参数后，程序读取参数获取keytab路径和prinpical再在程序中认证**

● 程序类型：支持 JAVA、Scala 和 Python 三种语言。

● 主函数的 class：是 Flink 程序的入口 Main Class 的全路径。

● 主 jar 包：是 Flink 的 jar 包。

● 部署方式：支持 cluster、local 两种模式。

● slot 数量：可以设置 slot 数。

● taskManage 数量：可以设置 taskManage 数。

● jobManager 内存数：可以设置 jobManager 内存数。

● taskManager 内存数：可以设置 taskManager 内存数。

● 命令行参数：是设置 Spark 程序的输入参数，支持自定义参数变量的替换。

● 其他参数：支持 --jars、--files、--archives、--conf 格式。

● 资源：如果其他参数中引用了资源文件，需要在资源中选择指定。

● 自定义参数：是 Flink 局部的用户自定义参数，会替换脚本中以${变量}的内容。

● 注意：JAVA 和 Scala 只是用来标识，没有区别，如果是 Python 开发的 Flink 则没有主函数的 class，其他都是一样。

### 8.6 Datax 节点

DataX组件使用目前支持自定义模板，利用json文件进行数据同步

● 单独说明：

① 各 [YARN集群router列表](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/blob/master/DolphinScheduler%E7%A4%BA%E4%BE%8B/YARN%E9%9B%86%E7%BE%A4router%E5%88%97%E8%A1%A8)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）

② keytab等认证文件在json文件中填写DS资源中心相对路径，并在datax节点页面“文件资源”下拉框处选择相应文件。

**下图是 hive 为数据源，postgresql 为目标库的 json 实例**
![](/img/task/DS图片/图片64.png)

● Json：[任务模版](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/blob/master/DolphinScheduler%E7%A4%BA%E4%BE%8B/data-hive-postgresql.json)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）

**下图是 hive 为数据源，clickhouse 为目标库的 json 实例**

![](/img/task/DS图片/图片65.png)

● Json：[任务模版](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/blob/master/DolphinScheduler%E7%A4%BA%E4%BE%8B/datax-odatatest-hive-ck)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）


## 9.参数

### 9.1 系统参数

| 变量                  | 含义                                                                   |
| --------------------- | ---------------------------------------------------------------------- |
| ${system.biz.date}    | 日常调度实例定时的定时时间前一天，格式为 yyyyMMdd，补数据时，该日期 +1 |
| ${system.biz.curdate} | 日常调度实例定时的定时时间，格式为 yyyyMMdd，补数据时，该日期 +1       |
| ${system.datetime}    | 日常调度实例定时的定时时间，格式为 yyyyMMddHHmmss，补数据时，该日期 +1 |

### 9.2 时间自定义参数

● 支持代码中自定义变量名，声明方式：${变量名}。可以是引用 "系统参数" 或指定 "常量"。

● 我们定义这种基准变量为 [...] 格式的，[yyyyMMddHHmmss] 是可以任意分解组合的，比如：$[yyyyMMdd], $[HHmmss], $[yyyy-MM-dd] 等

也可以使用以下格式：

● * 后 N 年：$[add_months(yyyyMMdd,12*N)]

● * 前 N 年：$[add_months(yyyyMMdd,-12*N)]

● \* 后 N 月：$[add_months(yyyyMMdd,N)]

● \* 前 N 月：$[add_months(yyyyMMdd,-N)]

● * 后 N 周：$[yyyyMMdd+7*N]

● * 前 N 周：$[yyyyMMdd-7*N]

● \* 后 N 天：$[yyyyMMdd+N]

● \* 前 N 天：$[yyyyMMdd-N]

● \* 后 N 小时：$[HHmmss+N/24]

● \* 前 N 小时：$[HHmmss-N/24]

● \* 后 N 分钟：$[HHmmss+N/24/60]

● \* 前 N 分钟：$[HHmmss-N/24/60]

### 9.3 用户自定义参数

● 用户自定义参数分为全局参数和局部参数。全局参数是保存工作流定义和工作流实例的时候传递的全局参数，全局参数可以在整个流程中的任何一个任务节点的局部参数引用。 例如：

![](/img/task/DS图片/图片68.png)

●global_bizdate 为全局参数，引用的是系统参数。

![](/img/task/DS图片/图片69.png)

● 任务中 local_param_bizdate 通过${global_bizdate}来引用全局参数，对于脚本可以通过。${local_param_bizdate}来引全局变量 global_bizdate 的值，或通过 JDBC 直接将 local_param_bizdate 的值 set 进去。

## 10.通过 TFS 启动 DS 任务流

通过 TFS 的 CI 执行 shell 脚本实现上传文件至 DS 资源中心以及运行 DS 任务流。

### 10.1 上传脚本至效能平台**git**库

#### 10.**1.1** 启动任务流脚本内容（点击下方链接复制内容或者下载均可直接使用）
[脚本地址](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/blob/master/DolphinScheduler%E7%A4%BA%E4%BE%8B/process.sh)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）

![demo启动](/img/task/DS图片/demo启动任务.png)

● 注意：token令牌值获取。
![](/img/task/DS图片/token.png)

● 注意：任务流 ID 在 DS 页面进入到⼀个工作流后，查看 url 地址后的数值。

![1](/img/task/TFS启动ds任务流图片/工作流ID.png)

● 注意：定时时间为工作流上线后，点击定时复制其中的定时时间格式即可。

![2](/img/task/TFS启动ds任务流图片/2.png)

#### 10.1.2 上传⾄**TFS**后检查脚本内容

![3](/img/task/TFS启动ds任务流图片/3.png)

● ⽣成脚本文件后进入效能平台代码-文件，在左侧选择路径上传脚本。

### 10.2 配置生成执行脚本

#### 10.2.1 启动**DS**工作流

![4](/img/task/TFS启动ds任务流图片/4.png)

● 点击版本和发布-生成-新建。

![5](/img/task/TFS启动ds任务流图片/5.png)

● 选择 TFS Git，点击继续。

![6](/img/task/TFS启动ds任务流图片/6.png)

● 点击空进程，进入生成配置页面。

![7](/img/task/TFS启动ds任务流图片/7.png)

● 选择代理队列后，在阶段 1 后加号⾥实用工具选择 Shell 脚本，点击添加。

![8](/img/task/TFS启动ds任务流图片/8.png)

● 脚本路径选择之前上传的脚本路径位置，选择启动⼯作流的对应脚本。

![9](/img/task/TFS启动ds任务流图片/9.png)

● 启动 DS 任务流：在参数中输⼊项目名称，token 值（图为示例，同上），任务流定义 ID 以及定时时间格式。

![10](/img/task/TFS启动ds任务流图片/10.png)

● 点击右上角保存和排队。

![11](/img/task/TFS启动ds任务流图片/11.png)

● 再点击右上角队列，将执行对应脚本对 DS 的操作，上传文件至 DS 资源中心以及启动任务流。流程结束后在 ds 平台相关页面查看是否执行成功。

### 10.3 查看执行结果

![12](/img/task/TFS启动ds任务流图片/12.png)

● 进⼊ DS 平台后，进⼊项⽬管理，进入工作流中的工作流实例，查看在 TFS 配置的⼯作流 id 所对应的任务是否启动，如已启动即为成功。

## 11.TFS 上传文件至 DS 资源中心

将脚本上传至效能平台 git 库，通过发布定义配置后执行脚本。

### 11.1 上传脚本至效能平台 git 库

#### 11.1 .1 上传文件脚本内容（点击下方链接复制内容或者下载均可直接使用）

[脚本地址](http://172.24.131.137:30080/odata_doc/odata_code_demo/-/blob/master/DolphinScheduler%E7%A4%BA%E4%BE%8B/upload.sh)（ **请登录[郑州资源池v5vpn](http://172.24.131.137:30080/root/doc-data/-/blob/master/data/%E5%A4%A7%E6%95%B0%E6%8D%AE/%E7%BD%91%E7%BB%9C%E9%9C%80%E6%B1%82%E7%94%B3%E8%AF%B7%E6%8C%87%E5%8D%97/README.md)后访问**）

![demo启动](/img/task/DS图片/demo传文件.png)

---

● 注意：token令牌值获取。
![](/img/task/DS图片/token.png)

● 注意：目录 ID 为 DS 资源中心点击进入文件夹后查看 url 最后位置的值。
![](/img/task/TFS启动ds任务流图片/目录1.png)

---

#### 11.1 .2 上传至 TFS 后检查脚本内容

![](/img/task/TFS启动ds任务流图片/上传脚本2.png)

● 生成脚本文件后进入效能平台代码-文件，在左侧选择路径上传脚本。（注意：上传脚本需要在下面上传至 ds 的文件在同一目录下）

### 11.2 配置生成执行脚本

#### 11.2.1 上传文件至 DS 资源中心目标文件夹

![](/img/task/TFS启动ds任务流图片/cd1.png)
● 点击版本和发布-发布-加号-创建发布定义。

![](/img/task/TFS启动ds任务流图片/cd2.png)

● 点击空进程。

![](/img/task/TFS启动ds任务流图片/cd3.png)

● 点击添加项目，右侧选择 Git，下方选择对应的源（存储库）以及默认分支，点击下方添加。

![](/img/task/TFS启动ds任务流图片/cd4.png)

● 点击环境 1 下查看环境任务，进入发布流程配置。

![](/img/task/TFS启动ds任务流图片/cd5.png)

● 点击代理阶段右侧加号选择实用工具-shell 脚本-添加。

![](/img/task/TFS启动ds任务流图片/cd13.png)

● 上传文件至 DS 资源中心：在参数中输入，用户自行在安全中心创建的 token 值（图中为示例），文件名，要上传的文件名，DS 的目标文件夹名称以及目录 id。（注意：需要将要传至 ds 的文件置于上传脚本相同路径下，即第 2 位与第 3 位参数一致，目标文件夹名称参数前加/，不可加后方。）

![](/img/task/TFS启动ds任务流图片/cd7.png)

● 点击保存后，点击右上角发布，选择环境 1，创建。

![](/img/task/TFS启动ds任务流图片/cd8.png)

● 再点击左上角定义名称，进入定义页面。

![](/img/task/TFS启动ds任务流图片/cd9.png)

● 点击操作-部署，点击弹窗中的部署，开始执行流程。

![](/img/task/TFS启动ds任务流图片/cd10.png)

● 点击日志查看流程运行情况，无异常保存即为运行成功。

### 11.3 查看执行结果

![](/img/task/TFS启动ds任务流图片/cd11.png)

● 进入平台后，进入资源中心对应文件夹，查看刚才从 TFS 平台上传的文件是否已经存在，如已存在即为成功。
