Spark Installation on Cloudera Quick Start VM
========

Spark 1.3.0 (default)
Python 2.6.6
Java 1.7

## Get ur weapon
```shell
wegt http://www.apache.org/dyn/closer.lua/spark/spark-1.5.2/spark-1.5.2-bin-hadoop2.6.tgz
tar zxvf spark-1.5.2-bin-hadoop2.6.tgz
```

## 
```shell
echo "export PATH=$PATH:/home/cloudera/spark-1.5.2-bin-hadoop2.6/bin" >> ~/.bashrc
```

## Set SPARK_HOME
```shell
export SPARK_HOME=/home/cloudera/spark-1.5.2-bin-hadoop2.6
```

## Copy hive-site.xml
```shell
cp /etc/hive/conf.dist/hive-site.xml $SPARK_HOME/conf
```
