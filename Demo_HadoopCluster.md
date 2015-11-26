# Hadoop Cluster on AWS

How to Set Up a Multi-Node Hadoop Cluster on Amazon EC2?

## Before we go
- NameNode (Master)
- SecondaryNameNode
- DataNode (Slave1)
- DataNode (Slave2)
And, 
- [Amazon AWS Account](http://aws.amazon.com/)
- Putty on Windows or Terminal on Mac

First, we'll create AWS instances and related infrastructure, then complete Hadoop multi-node cluster setup later.

## On AWS 
- Get Amazon AWS account with credit card
- Launch instance
- Select AMI, Ubuntu Server 12.04.3  Server 64-bit OS for example
- Select instance type
- Configure number of instances
- Add storage, min=8G
- Describe your instance
- Define a security group
- Launch instance and create security pair
- Launching instances and waiting for a while

## On command line
```shell
chmod 400 your.pem
ssh -i "your.pem" ubuntu@xx.xx.xx.xx
```
## Download Java 8
```shell
#nn, dn1, dn2
sudo apt-get install -y python-software-properties 
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```
Test it for safety!
```shell
java -version
jps
```
## Modify /etc/hosts
```shell
vim /etc/hosts
```
Then, add all machines' ip and hostname on it.

## Genkey for all machines
```shell
chmod 644 ~/.ssh/authorized_keys

$ scp -i ~/Downloads/starttech.pem ~/Downloads/starttech.pem ubuntu@54.254.213.15:~/
starttech.pem
cat .ssh/id_rsa.pub >>.ssh/authorized_keys

scp -i starttech.pem ~/.ssh/
ssh -i starttech.pem dn1 'cat ~/.ssh/id_rsa.pub' >> ~/.ssh/authorized_keys
cat ~/.ssh/id_rsa.pub | ssh -i starttech.pem dn1 'cat >> ~/.ssh/authorized_keys'
```

Test it,
```shell
ssh localhost
```

## Download Hadoop 
You may check all Hadoop version [here](http://ftp.twaren.net/Unix/Web/apache/hadoop/common/)

```shell
wget http://ftp.twaren.net/Unix/Web/apache/hadoop/common/hadoop-2.7.1/hadoop-2.7.1.tar.gz
```

Unzip the files.
```shell
tar -zxvf hadoop-2.7.1.tar.gz 
sudo mv hadoop-2.7.1 /usr/local
```

```shell
mv hadoop-1.2.1 hadoop
```

For simplicity, rename it.
```shell
vim ~/.profile
export HADOOP_HOME=/usr/local/hadoop-2.7.1
export PATH=$PATH:$HADOOP_HOME/bin

export HADOOP_PREFIX=/home/ubuntu/hadoop?
#Set JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-7-oracle?
# Add Hadoop bin/ directory to path
export PATH=$PATH:$HADOOP_PREFIX/bin
```

Repeat command above for remaining machines (dn1, dn2),
```shell
source ~/.bashrc
echo $HADOOP_PREFIX
echo $HADOOP_CONF
```

Config setting,
```shell
sudo apt-get install git
git clone https://github.com/youngce/hadoop-config.git
```

Check the hadoop version
```shell
hadoop version
```

hdfs-site.xml
hive-site.xml
yarn-site.xml

Modify slaves
```shell
vim slaves
+> dn1
+> dn2
-> localhost
```

vim hdfs-site.xml
vim core-site.xml
+> hadoop-2.7.1

scp -r /usr/local/hadoop-2.7.1 dn2:/usr/local
scp -r /usr/local/hadoop-2.7.1 dn1:/usr/local

```shell
hadoop namenode -format
```

GO!
```shell
#start dfs
$HADOOP_HOME/sbin/start-dfs.sh
```

If you'd like to stop service,
```shell
$HADOOP_HOME/sbin/stop-dfs.sh
```

Tips
```shell
ssh dn1 "rm -r /usr/local/hadoop-2.7.1/datanode.dir/"
```
