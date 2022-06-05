# hadoop

##

Hadoop is comprised of four main layers:

Hadoop Common is the collection of utilities and libraries that support other Hadoop modules.

- HDFS, which stands for Hadoop Distributed File System, is responsible for persisting data to disk.
  
- YARN, short for Yet Another Resource Negotiator, is the “operating system” for HDFS.
  
- MapReduce is the original processing model for Hadoop clusters. It distributes work within the cluster or map, then organizes and reduces the results from the nodes into a response to a query. Many other processing models are available for the 3.x version of Hadoop.
   Installation hadoop instruction
  
- Ubuntu 20.04/Debian
  

## install Java

let's first update the repository first

```
sudo apt update
```

then install **java**

```
sudo apt install default-jdk
```

check if it's worked by use this command

```bash
java -version; javac -version
```

fun then

## install ssh

for external access & authintace access

### install ssh server

```bash
apt install ssh -y
or
sudo apt install openssh-server openssh-client -y
```

let's then generate private key & public key

```bash
ssh-keygen -t ed25519
```

then authrise access without password for localhost

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

let's test it

```
ssh localhost
```

should access without need of password

## install hadoop

let's download from latest One by [hadoop.apache.org](https://hadoop.apache.org/releases.html)

let's choose [hadoop-3.3.3.tar.gz](https://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-3.3.3/hadoop-3.3.3.tar.gz)

first make sure at home directorly so

```bash
cd ~/
```

then    

```bash
wget -c https://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-3.3.3/hadoop-3.3.3.tar.gz
```

```bash
-c for continuance
```

check the chechsum sha512

> SHA512 (hadoop-3.3.3.tar.gz) = 1f5762682cef3daff8b2379fe7e40efca107bb7e8dcaa4a513e3bc0c082067759dd05d493ec997433dde2c89ea63dbc93aee0bba60045f89d3ec2d3f687f58b3

extract data at home directory

call the home directory

```bash
cd ~/
```

then extract the download hadoop by

```bash
tar -xzvf hadoop-3.3.1.tar.gz
```

move filolder to another directory

```bash
sudo mv hadoop-3.3.1 /usr/local/hadoop
```

## Configuring Hadoop’s Java Home

let's check first the loction of **java**

```bash
readlink -f /usr/bin/java | sed "s:bin/java::"
```

To begin, open `hadoop-env.sh`:

```bash
sudo nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
```

```bash
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
```

If you have trouble finding these lines, use `CTRL+W` to quickly search through the text. Once you’re done, exit with `CTRL+X` and save your file.

## test hadoop

```bash
/usr/local/hadoop/bin/hadoop
```

## add vars

```bash
sudo nano .bashrc
```

```bash
#Hadoop Related Options
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
```

active vars

```
source ~/.bashrc
```

### core-site.xml

```
sudo nano $HADOOP_HOME/etc/hadoop/core-site.xml
```

```tsconfig
<configuration>
<property>
  <name>hadoop.tmp.dir</name>
  <value>/home/hdoop/tmpdata</value>
</property>
<property>
  <name>fs.default.name</name>
  <value>hdfs://127.0.0.1:9000</value>
</property>
</configuration>
```

### hdfs-site.xml

```
sudo nano $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```

```rust
<configuration>
<property>
<name>dfs.data.dir</name>
<value>/home/hdoop/dfsdata/namenode</value>
</property>
<property>
<name>dfs.data.dir</name>
<value>/home/hdoop/dfsdata/datanode</value>
</property>
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
</configuration>
```

### Edit mapred-site.xml File

```
sudo nano $HADOOP_HOME/etc/hadoop/mapred-site.xml
```

```
<configuration>
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>
</configuration>
```

### Edit yarn-site.xml File

```
sudo nano $HADOOP_HOME/etc/hadoop/yarn-site.xml
```

```
<configuration>
<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
<property>
<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
<value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
<name>yarn.resourcemanager.hostname</name>
<value>127.0.0.1</value>
</property>
<property>
<name>yarn.acl.enable</name>
<value>0</value>
</property>
<property>
<name>yarn.nodemanager.env-whitelist</name>   
<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PERPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
</property>
</configuration>
```

## start hadoop

1. first format NameNode
  

```
hdfs namenode -format
```

2. start hadoop
  

```
./start-dfs.sh
```

3. start yarn
  
  ```
  ./start-yarn.sh
  ```
  

check if all done

```
jps
```

then try access web browser by

```
http://localhost:9870
```
