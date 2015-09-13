
```sh
    yum -y update
    yum install wget
    systemctl stop firewalld


    [기존 java 삭제하기]

    yum -y remove "java-*"
```

    1. jdk 다운로드

```sh
    cd ~/Downloads

    wget —no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz

    cd ~/Downloads/
    tar –zxvf jdk-8u5-linux-x64.tar.gz
    mkdir /usr/java
    mv jdk1.8.0_60 /usr/java/jdk1.8

    vi /etc/profile

    밑에 추가

    export JAVA_HOME=/usr/java/jdk1.8
    export PATH=$JAVA_HOME/bin:$PATH
    export CLASSPATH=$CLASSPATH:$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar

    source /etc/profile
```

    2. 계정 추가

```sh
    useradd hadoop
    su - hadoop
    ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
    cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys
```

    3. 로컬호스트 들어갔다 나오기

```sh
    ssh localhost
    exit
    (ctrl + d)
```

    4. 하둡다운로드
    
```sh
    wget http://apache.tt.co.kr/hadoop/common/hadoop-2.7.1/hadoop-2.7.1.tar.gz
    tar -zxvf hadoop-2.7.1.tar.gz
```

    5. 컨피규어링

```sh
    $vi $HOME/.bashrc
    export JAVA_HOME=/usr/java/jdk1.8
    export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
    export HADOOP_HOME=/home/hadoop/hadoop-2.7.1
    export HADOOP_INSTALL=$HADOOP_HOME
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    export HADOOP_COMMON_HOME=$HADOOP_HOME
    export HADOOP_HDFS_HOME=$HADOOP_HOME
    export HADOOP_YARN_HOME=$HADOOP_HOME
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
    export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
    export JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH

    $source $HOME/.bashrc
```

    6. 속성값 추가하기 : <configuration> </configuration> 사이에

```sh
    vi $HADOOP_HOME/etc/hadoop/core-site.xml

    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
```

-------------------------------------------------

```sh
    vi $HADOOP_HOME/etc/hadoop/hdfs-site.xml

    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
 
    <property>
        <name>dfs.name.dir</name>
        <value>file:///home/hadoop/hadoopdata/hdfs/namenode</value>
    </property>
 
    <property>
        <name>dfs.data.dir</name>
        <value>file:///home/hadoop/hadoopdata/hdfs/datanode</value>
    </property>
```

-------------------------------------------------

```sh

    cp $HADOOP_HOME/etc/hadoop/mapred-site.xml.template $HADOOP_HOME/etc/hadoop/mapred-site.xml
    
```
-------------------------------------------------

```sh

    vi  $HADOOP_HOME/etc/hadoop/mapred-site.xml

    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
```

-------------------------------------------------

```sh
    vi $HADOOP_HOME/etc/hadoop/yarn-site.xml

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
```

-------------------------------------------------

```sh
    set JAVA_HOME
```

-------------------------------------------------

```sh
    vi $HADOOP_HOME/etc/hadoop/hadoop-env.sh 
    export JAVA_HOME=/usr/java/jdk1.8
```

------------------------------------------
    7. 하둡시작
    
```sh
    hdfs namenode -format
    start-dfs.sh
    start-yarn.sh
```
    8. Web GUI 확인

```sh    
    localhost:50070
```
    9. put a file ~

```sh    
    $ hdfs dfs -mkdir /user
    $ hdfs dfs -mkdir /user/hadoop
    $ hdfs dfs -put /var/log/boot.log
```
