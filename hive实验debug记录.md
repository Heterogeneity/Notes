# 实验背景

大三下大数据分析实验四，实验内容为hive操作和滴滴打车数据分析项目。实验全部在root用户下进行。在实验过程中，出现一系列错误，现已全部修复，特将具体debug过程记录下来。

# Debug过程描述

- 问题：root用户运行`start-all.sh`脚本，显示没有权限

  原因：之前跟着黑马那个视频安装Hadoop时将hadoop文件的权限分给了hadoop用户

  解决方式：运行`chown -R root:root /data`和`chown -R root:root /export`将权限改回给root。由于是测试环境，为了方便就把所有的实验相关文件全给了root，实际生产环境下应该把权限分配给非root用户，防止对系统的修改。

- 问题：运行hive时，vmware报错，大概意思是写磁盘失败，虚拟机磁盘损坏

  原因：之前快照拍的太多了，导致虚拟机的磁盘空间被快照占满

  解决方式：删去多余快照，重启虚拟机

- 问题：`beeline`连接到10000端口时失败，`jps`显示`hiveserver2`虽然启动但10000端口没有监听

  原因：经过查看hive日志排查，发现有两个报错，一个是hdfs的安全模式被打开，无法创建文件，另一个是`HA not enabled`

  解决方式：运行`hdfs dfsadmin -safemode leave`(或`hdfs dfsadmin -safemode forceexit`强制退出)离开hdfs安全模式；

  在hive配置文件下添加`hive.server2.active.passive.ha.enable`值为`true`

  注意事项：可以在hdfs的网页服务端口查看safemode的自动启动原因，在退出安全模式后最好先修复问题再进行读写操作

- 问题：找不到hive日志文件

  原因：hive的`conf`文件夹下`hive-log4j2.properties`文件中的`property.hive.log.dir`制定了hive日志的路径，需要修改该路径的默认值才能在修改后的路径下找到`hive.log`文件

  解决方式：修改`hive-log4j2.properties`文件中的`property.hive.log.dir`，到该路径下寻找hive.log查看日志信息

  注意事项：`hive-log4j2.properties`由`hive-log4j2.properties.template`运行`mv`命令获得

- 问题：在beeline中运行命令，到8088端口查看，无法找到相关任务的运行状态

  原因：之前只把node1的`/export`和`/data`文件权限分配给了root用户，node2和node3没有这样操作，导致除hadoop用户外的用户无法在8088端口上查看到任务执行状态

  解决方式：在node2和node3上运行`chown -R root:root /data`和`chown -R root:root /export`将权限改回给root。为了更保险一点，在node1、node2和node3的Hadoop配置文件`yarn-site.xml`中添加`mapreduce.framework.name`属性，值为`yarn`

- 问题：在IDEA中运行insert相关命令，返回`code2`报错

  原因：到8088端口查看报错，发现找不到`hadoop classpath`的值，报错建议到hadoop配置文件设置属性，值为Hadoop安装路径

  解决方式：node1、node2和node3的hadoop配置文件夹下的`mapred-site.xml`添加`yarn.app.mapreduce.am.env`、`mapreduce.map.env`、`mapreduce.reduce.env`三个属性，值均为`HADOOP_MAPRED_HOME=hadoop安装路径`

- 问题：insert命令仍然无法运行，想在8088端口下找到任务的具体日志内容，拒绝访问

  原因：没有开启日志聚合功能

  解决方式：在hadoop配置文件夹下的`yarn-site.xml`中添加`yarn.log-aggregation-enable`、`yarn.nodemanager.remote-app-log-dir`属性，值分别为`true`和自定义的日志保存路径(在hdfs下)

- 问题：在hdfs中查找刚刚指定的日志保存路径，找到insert失败任务日志，发现报错为`mapreduce-shuffle`没开启

  原因：未配置该属性

  解决方式：在hadoop配置文件就下的`yarn-site.xml`中添加`yarn.nodemanager.aux-services`属性，值为`mapreduce_shuffle`

  至此，insert操作可以成功执行

- 问题：在debug过程中，出现启动hadoop集群时个别节点的`nodemanager`或`datanode`节点启动失败的错误，以及关闭集群出现`did not stop gracefully after 5 seconds`的警告

  原因：节点之间的配置文件内容不一致或有错误，具体可到出现问题的节点的hadoop文件夹下的`logs`文件夹下查看具体启动日志

  解决方式：往往是单词拼写错误导致启动出错，根据日志提示改正即可解决启动节点失败的问题；另外保证节点的配置文件内容一致，可以挨个排查，以解决关闭出现警告的问题
