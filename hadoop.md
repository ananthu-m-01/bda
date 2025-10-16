# Hadoop Installation and Configuration (Single-Node Setup)

## Table of Contents

* [Prerequisites](#prerequisites)
* [Step 1: Install Java](#step-1-install-java)
* [Step 2: Download and Extract Hadoop](#step-2-download-and-extract-hadoop)
* [Step 3: Configure Environment Variables](#step-3-configure-environment-variables)
* [Step 4: Configure Hadoop XML Files](#step-4-configure-hadoop-xml-files)
* [Step 5: Setup SSH](#step-5-setup-ssh)
* [Step 6: Format HDFS](#step-6-format-hdfs)
* [Step 7: Start Hadoop Services](#step-7-start-hadoop-services)
* [Step 8: Verify Installation](#step-8-verify-installation)
* [Step 9: Stop Hadoop](#step-9-stop-hadoop)
* [Web Interfaces](#web-interfaces)

---

## Prerequisites

* OS: Linux (Ubuntu 20.04+ recommended) or macOS
* RAM: Minimum 4 GB (8 GB recommended)
* Java: JDK 8 or 11 installed

---

## Step 1: Install Java

```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
java -version
```

Set environment variables:

```bash
nano ~/.bashrc
```

Add:

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
```

Apply:

```bash
source ~/.bashrc
```

---

## Step 2: Download and Extract Hadoop

```bash
cd /usr/local
sudo wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
sudo tar -xzvf hadoop-3.3.6.tar.gz
sudo mv hadoop-3.3.6 hadoop
```

---

## Step 3: Configure Environment Variables

```bash
nano ~/.bashrc
```

Add:

```bash
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
```

Apply changes:

```bash
source ~/.bashrc
```

---

## Step 4: Configure Hadoop XML Files

### 1. `hadoop-env.sh`

```bash
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
```

Add:

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

### 2. `core-site.xml`

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

### 3. `hdfs-site.xml`

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/usr/local/hadoop_data/hdfs/namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/usr/local/hadoop_data/hdfs/datanode</value>
    </property>
</configuration>
```

### 4. `mapred-site.xml`

```bash
cp mapred-site.xml.template mapred-site.xml
nano mapred-site.xml
```

Add:

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

### 5. `yarn-site.xml`

```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

---

## Step 5: Setup SSH

```bash
sudo apt install ssh
ssh-keygen -t rsa -P ""
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
ssh localhost
```

---

## Step 6: Format HDFS

```bash
hdfs namenode -format
```

---

## Step 7: Start Hadoop Services

```bash
start-dfs.sh
start-yarn.sh
```

---

## Step 8: Verify Installation

```bash
jps
```

You should see:

```
NameNode
DataNode
SecondaryNameNode
ResourceManager
NodeManager
```

---

## Step 9: Stop Hadoop

```bash
stop-yarn.sh
stop-dfs.sh
```

---

## Web Interfaces

| Service         | URL                                            |
| --------------- | ---------------------------------------------- |
| NameNode        | [http://localhost:9870](http://localhost:9870) |
| ResourceManager | [http://localhost:8088](http://localhost:8088) |

---

This README provides **complete steps to install and configure Hadoop** in a pseudo-distributed single-node setup.
