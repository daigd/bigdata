### 下载链接

> http://archive.cloudera.com/cdh5/

>cdh5基于官网整合了hadoop相关资源的版本差异性,用统一的cdh版本号来管理各个资源,优先使用该链接下载。

> http://hadoop.apache.org/

> 官网下载链接。

### hdfs 操作命令
创建目录
> `bin/hdfs dfs -mkdir -p dfs文件系统目标目录`,
> 示例: `bin/hdfs dfs -mkdir -p /user/dgd/tmp`。

上传文件
> `bin/hdfs dfs -put 文件源目标  hdfs文件系统目标目录`,
> 示例: `bin/hdfs dfs -put /opt/modules/hadoop-2.8.5/etc/hadoop/core-site.xml /user/dgd/tmp/`。

读文件
> `bin/hdfs dfs -text hdfs文件系统的目标文件路径`,
> 示例: `bin/hdfs dfs -text /user/dgd/tmp/core-site.xml` 。

都验证没问题表明Hadoop hdfs读写功能正常。