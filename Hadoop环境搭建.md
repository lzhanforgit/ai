# Hadoop-2.8.0 开发环境搭建（Mac）
1. 简介

    Hadoop是一个由Apache基金会开发的分布式系统架构，简称HDFS，具有高容错性、可伸缩性等特点，并且可以部署在低配置的硬件上；同时，提供了高吞吐量的数据访问性能，适用于超大数据集的应用程序，以及通过集群拓扑高效快速的处理数据的能力。

2. 下载源码

    在搭建环境之前，需要先下载hadoop的binary，可以把 source也下载下来，方便以后阅读。下载后进行解压：
    
    ```
        $ tar -zxvf hadoop-2.8.0.tar.gz
    ```

3. 配置

    添加的方式很多，可以修改系统级的文件，如：/etc/bashrc、/etc/profile，也可以修改当然用户的文件，比如：~/.bash_profile（shell用的是bash）、~/.zshrc（shell用的是zsh），添加如下代码即可
    
    ```
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home
export HADOOP_HOME=/Users/gandalf/Documents/kelvin/Hadoop/hadoop-2.8.0
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

    ```
    
    配置完成后，需要设置其生效，安装zsh的配置设置：
    
    ```
    $ source ~/.zshrc
    ```
设置完成后，如果出现下面信息，表示hadoop开发环境变量设置好了

    ```
$ hadoop version
Hadoop 2.8.0
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r 91f2b7a13d1e97be65db92ddabc627cc29ac0009
Compiled by jdu on 2017-03-17T04:12Z
Compiled with protoc 2.5.0
From source with checksum 60125541c2b3e266cbf3becc5bda666
This command was run using /Users/gandalf/Documents/kelvin/Hadoop/hadoop-2.8.0/share/hadoop/common/hadoop-common-2.8.0.jar
    ```

4. 修改hadoop-env.sh
直接设置JAVA_HOME的路径，不要用$JAVA_HOME代替，因为hadoop对系统变量的支持不是太好

    ```
        # The java implementation to use.
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home
    ```
    
5. 修改core-site.xml


    设置hadoop的临时目录及文件系统，其中localhost:9000表示本地主机，如果是远程主机，则需要把localhost修改为相应的IP地址，如果填写远程主机的域名，则需要到/etc/hosts文件中做DNS映射

    ```
    <configuration>
        <!--设置临时目录-->
        <property>
            <name>hadoop.tmp.dir</name>
            <value>/Users/gandalf/Documents/kelvin/Hadoop/hadoop-2.8.0/data</value>
        </property>
        <!--设置文件系统-->
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://localhost:9000</value>
        </property>
    </configuration>
    
    ```
4. 配置hdfs-site.xml
由于是一台Mac电脑，所以数据的副本设置为1，默认是3

    ```
    <configuration>
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
    </configuration>
    ```
5. 配置mapred-site.xml

    由于hadoop的根目录下的etc/hadoop目录下没有mapred-site.xml文件，所以需要创建该文件，但是我们可以直接把etc/hadoop目录下的mapred-site.xml.template文件重命名为mapred-site.xml，然后配置数据处理的框架为yarn。

    ```
    <configuration>
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
    </configuration>
    ```
6. 配置yarn-site.xml
    配置数据处理框架yarn
    
    ```
    <configuration>
    
    <!-- Site specific YARN configuration properties -->
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        <property>
            <name>yarn.resourcemanager.address</name>
            <value>localhost:9000</value>
        </property>
    </configuration>
    
    ```
----
# 启动hadoop

配置完hadoop后，就可以启动hadoop了

1. 启动namenode
    $ hadoop namenode -format
    如果出现如下图片提示，表示namenode启动成功
    
    图1
    
    ![](https://upload-images.jianshu.io/upload_images/1843940-4c87fd5c489c48dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
    需要注意的是，如果有错误，请先检查是不是hadoop安装包是32-bit，而计算机系统是64-bit，造成不匹配

2. 启动hdfs
    启动hdfs，有可能需要输入3次密码
    
    $ start-dfs.sh
    如果出现如下提示，表示hadoop无法远程登录主机，需要开放权限
    ![](https://upload-images.jianshu.io/upload_images/1843940-ac8654b008736148.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
    图2
    具体开放权限的步骤如下，先到系统设置里的共享里，然后允许远程登录，最后添加当前的用户即可：
    
3. 启动yarn
    
    启动数据处理mapreduce框架yarn

    ```
    $ start-yarn.sh
    ```
    
    如果执行jps命令出现如下提示，表示hadoop启动完成

    ![](https://upload-images.jianshu.io/upload_images/1843940-493f3da8eb7ff717.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/205)

4. 浏览器查看hadoop
我们也可以通过浏览器查看hadoop的详细信息，打开链接：

    ```
    http://localhost:50070/
    
    ```
    ![](https://upload-images.jianshu.io/upload_images/1843940-e2e3802d0838146e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)


