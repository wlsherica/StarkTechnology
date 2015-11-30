Spark Installation on Cloudera Quick Start VM
========

- Spark 1.3.0 (default)
- Python 2.6.6
- Java 1.7

### Spark 1.4.1
## Get ur weapon

Spark [download](http://spark.apache.org/downloads.html)
```shell
# this will download a compressed TAR file
wget http://d3kbcqa49mib13.cloudfront.net/spark-1.4.1-bin-hadoop2.6.tgz
# untar the file
tar zxvf spark-1.4.1-bin-hadoop2.6.tgz
```
## 
```shell
#  set path variable
echo "export PATH=$PATH:/home/cloudera/spark-1.4.1-bin-hadoop2.6/bin" >> ~/.bashrc
```
## Set SPARK_HOME
```shell
export SPARK_HOME=/home/cloudera/spark-1.4.1-bin-hadoop2.6
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
data.take(3)
```

### Spark 1.3.0 
## Connected hdfs folder

Upload spark example data to HDFS (default)
```shell
hadoop fs -put /home/cloudera/spark-1.4.1-bin-hadoop2.6/examples/src/main/resources/people.txt .
```
Test it in pyspark
```python
file = sc.textFile("/user/cloudera/people.txt")
file.take(1)
```

## Set standalone cluster
```shell
cd /usr/lib/spark
# modify slave
vim conf/slaves
```
Set config
```shell
cd /etc/alternatives/spark-conf
sudo vim conf/spark-env.sh

export SPARK_WORKER_CORES=1
export SPARK_WORDER_INSTANCES=2
export SPARK_WORKER_MEMORY=2g
```
Then launch master and slave
```shell
cd /usr/lib/spark
sudo /etc/init.d/spark-worker restart
# launch master and slave
sudo sbin/start-all.sh
```
Master URL: http://localhost:18081
```shell
# check the local VM identifier
sudo jps
```
How to submit job with cluster?
```shell
sudo ./bin/spark-submit --master spark://quickstart.cloudera:7077 ~/spark-1.4.1-bin-hadoop2.6/example/src/main/python/pi.py
```
How to submit job without cluster?
```shell
./bin/spark-submit ~/spark-1.4.1-bin-hadoop2.6/example/src/main/python/pi.py
```
## Connect with Cluster Master
```shell
bin/spark-shell --master spark://quickstart.cloudera:7077
```
