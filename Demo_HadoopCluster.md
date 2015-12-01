# Hadoop Cluster on AWS

How to Set Up a Multi-Node Hadoop Cluster on Amazon EC2?

## Before we go
- NameNode (master)
- SecondaryNameNode
- DataNode (slave1)
- DataNode (slave2)
- [Amazon AWS Account](http://aws.amazon.com/)
- Putty on Windows or Terminal on Mac

First, we'll create AWS instances and related infrastructure, then complete Hadoop multi-node cluster setup later.

## On AWS 
- Get Amazon AWS account with credit card
- Launch instance
- Select AMI, Ubuntu Server 14.04 LTS (HVM), SSD Volume Type for example
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
## Download Java 7 on all machines
```shell
#nn, dn1, dn2
sudo apt-get install -y python-software-properties 
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install openjdk-7-jdk
# this is for java8
#sudo apt-get install oracle-java8-installer
```
Test it for safety!
```shell
java -version
jps
```
## Modify /etc/hosts
```shell
sudo vim /etc/hosts
```
Then, add all machines' ip and hostname on it.

## Genkey for all machines
```shell
chmod 644 ~/.ssh/authorized_keys
# copy .pem from local system to nn 
scp -i ~/Downloads/your.pem ~/Downloads/your.pem ubuntu@<nn-public-ip>:~/

cp ~/StarkTech.pem ~/.ssh/

# create the public fingerprint on namenode
ssh-keygen -f ~/.ssh/id_rsa -t rsa -P ""
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

#
scp -i ~/.ssh/StarkTech.pem ~/.ssh/StarkTech.pem ubuntu@<dn1-public-ip>:~/.ssh/

# copy the public fingerprint to each datanodes
cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/your.pem dn1 'cat >> ~/.ssh/authorized_keys'
cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/your.pem dn2 'cat >> ~/.ssh/authorized_keys'
```

Test it,
```shell
ssh localhost
ssh dn1
ssh dn2
```

## Download Hadoop then install Hadoop onto all the nodes
You may check all Hadoop version [here](http://ftp.twaren.net/Unix/Web/apache/hadoop/common/)

```shell
# at nn
cd ~
wget http://ftp.twaren.net/Unix/Web/apache/hadoop/common/hadoop-2.7.1/hadoop-2.7.1.tar.gz
scp ~/hadoop-2.7.1.tar.gz dn1:~/
scp ~/hadoop-2.7.1.tar.gz dn2:~/
```

Unzip the files.
```shell
tar -zxvf hadoop-2.7.1.tar.gz 
sudo mv hadoop-2.7.1 /usr/local
```

```shell
cd /usr/local
sudo mv hadoop-2.7.1 hadoop
```

For simplicity, rename it.
```shell
vim ~/.profile
```
Modify these variables
```shell
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export HADOOP_PREFIX=/usr/local/hadoop
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop

#Set JAVA_HOME
#java-8-oracle for java8
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
# Add Hadoop bin/ directory to path
export PATH=$PATH:$HADOOP_PREFIX/bin
```
How to find JAVA path?
```shell
whereis java
ll /usr/bin/java
ll /etc/alternatives/java
# Voila! 
```

Repeat command above for remaining machines (dn1, dn2),
```shell
source ~/.profile
echo $HADOOP_PREFIX
```

Config setting,
```shell
cd /usr/local/hadoop/etc
sudo apt-get install git
git clone https://github.com/wlsherica/StarkTechnology.git
mv StarkTechnology/hadoop-config/ .
rm -rf StarkTechnology
```
Move hadoop-config
```shell
mv hadoop hadoop_ori
mv hadoop-config hadoop
```

Make sure all conf files scp to all machines
```shell
# add JAVA_HOME
vim hadoop-env.sh
# dfs.replication=3, dfs.permissions.enabled = false
vim hdfs-site.xml
# check fs.default.name
vim core-site.xml 
```

Check the hadoop version
```shell
hadoop version
```

Modify slaves
```shell
vim slaves
+> dn1
+> dn2
-> localhost
```

Move configuration files to datanodes, note that masters file on slave machine is going to be empty, and the ‘slaves’ file at datanode contains only its own IP and not of any other datanode in the cluster.

```shell
scp -r /usr/local/hadoop dn2:/usr/local
scp -r /usr/local/hadoop dn1:/usr/local

scp ~/.profile dn1:~/
scp ~/.profile dn2:~/
```
Format the namenode 
```shell
hadoop namenode -format
```
GO!
```shell
#start dfs
$HADOOP_HOME/sbin/start-dfs.sh
```
Check it out
```shell
jps
ssh dn1 'jps'
ssh dn1 'jps'
```
If you'd like to stop service,
```shell
$HADOOP_HOME/sbin/stop-dfs.sh
```
## Tips
```shell
ssh dn1 "rm -r /usr/local/hadoop-2.7.1/datanode.dir/"
```
```shell
hadoop fsck /
```
## Start up YARN
```shell
$HADOOP_HOME/sbin/start-yarn.sh
```
## Tracking URL
- Namenode status http://publicDNS:50070/dfshealth.jsp
- Monitor each job by: http://publicDNS:8088/cluster
