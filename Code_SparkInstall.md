Spark Installation on Cloudera Quick Start VM
========

Spark 1.3.0 (default)
Python 2.6.6
Java 1.7

### Spark 1.4.1
## Get ur weapon
```shell
wegt http://www.apache.org/dyn/closer.lua/spark/spark-1.4.1/spark-1.4.1-bin-hadoop2.6.tgz
tar zxvf spark-1.4.1-bin-hadoop2.6.tgz
```

## 
```shell
echo "export PATH=$PATH:/home/cloudera/spark-1.4.1-bin-hadoop2.6/bin" >> ~/.bashrc
```

## Set SPARK_HOME
```shell
export SPARK_HOME=/home/cloudera/spark-1.5.2-bin-hadoop2.6
```

## Copy hive-site.xml
```shell
sudo cp /etc/hive/conf.dist/hive-site.xml $SPARK_HOME/conf
```

## Find Hadoop HDFS path
```shell
ls /etc/alternatives/hadoop-conf/core-site.xml
```

## Test your hdfs path in pyspark
```python
data = sc.textFile("hdfs://quickstart.cloudera:8020/user/cloudera/people.txt")
```

### Spark 1.3.0 
# Connected hdfs folder

Upload spark example data to HDFS (default)
```shell
hadoop fs -put /home/cloudera/spark-1.5.2-bin-hadoop2.6/examples/src/main/resources/people.txt .
```
Test it in pyspark
```python
file = sc.textFile("/user/cloudera/people.txt")
file.take(1)
```
