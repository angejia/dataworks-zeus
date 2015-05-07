﻿Hadoop任务的调试运行到生产任务的周期调度
宙斯支持任务的整个生命周期

从功能上来说，支持：  
Hadoop MapReduce任务的调试运行  
Hive任务的调试运行  
Shell任务的运行    
Hive元数据的可视化查询与数据预览  
Hadoop任务的自动调度  

V0.15修改内容：
	1. 修改邮件发送bug,增加多个邮件发送人
	2. 修改Job自动开启关闭功能
	3. Noc报警配置写到配置文件
	4. JOB文件夹增加写的权限

v0.15.1
	增加自动开启关闭依赖检测。下游依赖存在开启，任务不能关闭。上游依赖全部开启，任务才能开启。
	从界面上移除了周期调度
	增加了历史action从内存中清除，并在数据库中备份，默认设置为两个月前的

v0.16修改漏跑程序

v0.18.1
修改默认依赖周期为同一天
添加双击任务栏可以跳到job详情功能
添加job任务总览按日期范围进行过滤功能

v0.19
优化依赖job的action方法
修复了pagingtoolbar的空指针和显示异常
zeus密码显示加密
修复work断线任务重新分配bug
修复指定不存在host导致队列阻塞bug
修复了依赖host不能指定的bug
修复了时间截取的bug

v0.20
增加job和action资源，脚本和目录同步更新功能
修改action生成时间，改为整点生成
JOB时间变更，action生成做调整：
	1. 情况一：job定时修改前和修改后都在下一次action生成之前，当天有两个版本，后生成的版本status=failed，漏跑检查时不执行。
	2. 情况二：job定时修改前在下一次action生成之前，修改后在下一次action生成之后，当天有两个版本，修改后的时间在schedule中继续等待调度。
	3. 情况三：job定时修改前和修改后都在下一次action生成之后，当天只有一个版本，删除修改前的时间版本。
	4. 情况四：job定时修改后在下一次action生成之前，修改前在下一次action生成之后，当天只有一个版本，删除修改前的时间版本，后生成的版本status=failed，不会漏跑调度。
	
v0.21
增加host修改即时生效功能.
增加hive元数据展示功能,暂时不支持rcfile的展示。
修复hive分区数据下载bug，改为访问hdfs api之前同时加载core-site.xml和hdfs-site.xml.
屏蔽系统配置项"zeus.dependency.cycle"; "run.priority.level";"roll.back.times";"roll.back.wait.time"。"zeus.secret.script"配置会展现但是不能够通过配置栏编辑。
修复了漏跑host指定的bug.
修复了漏发邮件缓存刷新的bug.

v0.22
增加Alarm模块的日志信息，方便日志查询。
修改NOC Alarm规则，NOC接口要求去掉\和[]符号。
组不从数据库删除，而将组标志位置为0。(exisited为1表示组存在)
任务开启/关闭修改时，过时任务不会漏跑执行。

v0.23
增加了关注人功能
增加了对自动调度任务的job延时报警功能
增加了单机安装脚本
修改了报表规则

v0.24
增加日志功能
修正了超时检测空指针异常
调整了重试最后一次noc

v0.25(补丁)
1.	DB连接池连接数配置修改，原先是15，现在改为50，从MySql服务器的访问连接看，正常是60-80。
2.	线程TimeOutException时间改为10s。
3.	 凌晨减少生成action版本的频率，0点生成一天的action，1:15分再生成一次（确保当天的action全部生成），
2-8点不生成，8点以后半小时生成一次，且避开整点和半点（大量JOB都在这个点调度）。
4.	用户访问日志暂时屏蔽，减轻数据库的负载压力。
5.	漏跑检测修改：当前时间10分钟以前的JOB做漏跑检测，避免quartz调度延时重复调度的情况。
6.	quarz定时器中已经存在的JOB不做更新。

v0.26
1.增加定时表达式界面
2.取消定时表达式界面中秒单位
3.hive出错不再重调
4.修复发送日志刷新大对象
5.增长抢master时间为5分钟
6.修复失败任务重试无状态的bug	
7.增加延迟报警的报警信息	
8.修复开发中心调试脚本日志超长，更新失败，导致队列中无法取消的问题

v0.27
1.用户帐号自助管理:用户可以注册帐号、修改帐号信息（包括email、noc联系人信息等）；管理员负责审核，并邮件通知DI团队开通hive帐号和权限。
2.报警邮件标题中加上Job名称。
3.Job的管理员有执行Job的权限。
4.创建组帐号的同时，创建组目录。
5.JOB增加重要联系人告警设置。
6.首页可以显示用户uid。
7.UI风格改版

v0.28
1.增加了复制其它任务的依赖到本任务的功能。
2.修复了任务总览下依赖状态显示错误问题。
3.修复了历史任务显示数目问题。
4.增加quartz配置。
5.增加zeus集群分组功能。
6.增加master抢占组。
7.注册页面增加说明。

v0.29
1.针对404部分问题进行修复
2.优化了操作界面

v0.30
1.增加调度分配策略：CPU和内存综合判断 
2.高优先级任务当指定组中host不存在，可能造成的队列堵塞问题   
3.修改了env.sh配置
4.修复了与Host组相关的一些bug。
5.增加开发中心datax配置工具

v0.31
1.修复了与Host组相关的一些bug.
2.取消了手动队列的优先级