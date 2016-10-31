# HDFS 完全分布式安装

## 开发环境
  - Vmware
  - Centos

## 思路
   - 设置三个节点 -- node1,node2,node3,node3,node4

   - namenode -- node1

   - secondarynode -- node2

   - Datanode -- node3,node4

## 步骤
  - 准备软件包 [hadoop-2.5.2.tar](http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.5.2/hadoop-2.5.2.tar.gz)

  - 解压软件包

  - 设置ssh免密码登陆
    >


  - 修改etc/hadoop/core-site.xml
    ```
    <configuration>
      <property>
        <name>fs.defaultFS</name>
        <value>hdfs://node1:9000</value>
      </property>
    </configuration>
    ```

  - 修改etc/hadoop/hdfs-site.xml:
  ```
  <configuration>
      <property>
        <name>dfs.replication</name>
        <value>3</value>
      </property>
  </configuration>
  ```

  - 修改masters文件和slaves文件


  - 格式化namenode

  - Start-hdfs.sh启动
