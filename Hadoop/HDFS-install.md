## HDFS 完全分布式搭建

### 开发环境
  - Vmware
  - Centos

### 思路
   - 设置三个节点 -- node1,node2,node3,node3,node4

   - NameNode -- node1

   - SecondaryNode -- node2

   - DataNode -- node3,node4

### 步骤
  - 关闭防火墙

   ```
   $ service iptables stop

   ```
   >开关防火墙法
        * 重启后生效
        开启： chkconfig iptables on
        关闭： chkconfig iptables off

   >     * 即时生效，重启后失效
        开启： service iptables start
        关闭： service iptables stop

  - 修改/etc/hosts文件  (根据实际情况修改Ip)

    在/etc/hosts文件中添加如下内容
    ```
    192.168.2.11 node1
    192.168.2.12 node2
    192.168.2.13 node3
    192.168.2.14 node4
    ```

  - 设置ssh免密码登陆
    ```
    $ ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
    $ cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys

    ```

    可写脚本批量解决问题

  - 修改hadoop-env.sh中的java环境变量

  - 修改etc/hadoop/core-site.xml
    ```
    <configuration>
       <property>
           <name>fs.defaultFS</name>
           <value>hdfs://node1:9000</value>
        </property>
        <property>
           <name>hadoop.tmp.dir</name>
           <value>/opt/hadoop</value> #确认该文件目录不存在 存在会出问题。
       </property>
   </configuration>
    ```

  - 修改etc/hadoop/hdfs-site.xml:
  ```
  <configuration>
       <property>
          <name>dfs.replication</name>
          <value>3</value> #最小副本数
       </property>
       <property>
          <name>dfs.namenode.secondary.http-address</name>
          <value>node2:50090</value>
       </property>
       <property>
          <name>dfs.namenode.secondary.https-address</name>
          <value>node2:50091</value>
       </property>
</configuration>
  ```
  
  - 修改masters文件和slaves文件
    masters文件中写入SecondaryNode 节点名称
    slaves文件中写入从节点（node2,node3,node4名称）
    ```
    $ mkdir etc/hadoop/masters
    $ mkdir etc/hadoop/slaves
    $ cat node2 >> etc/hadoop/masters
    $ cat node2,node3,node4 >> ect/hadoop/slaves

    ```

  - 格式化namenode
  ```
  $ bin/hdfs namenode -format

  ```


  - Start-hdfs.sh启动
  ```
  $ sbin/start-dfs.sh
  ```

  - 浏览器查看NN,SNN工作状态
    * NN  - http://node1:50070/
    * SNN - http://node2:50090/
    * web访问端口 客户端请求端口
      ```
      $ netstat -nplt | grep java
      ```

  - 创建HDFS目录
  ```
  $ bin/hdfs dfs -mkdir /user
  $ bin/hdfs dfs -mkdir /user/<username>
  ```
  - 运行例子（前提MapReduce已经启动）

  ```
  $ bin/hadoop jar packageName+className
  ```

- Stop-dfs.sh 关闭
  ```
   $ sbin/stop-dfs.sh
  ```
