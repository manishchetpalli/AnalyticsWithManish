

This setup includes Apache Spark cluster standalone installation and integration with hadoop on 3 Nodes.

### **Install Java**

Install java on all 3 machines.

### **Configure Host Files**

Configure host names and host files for each node.

Open the hosts file and Add IP addresses and hostnames of each node in the cluster.

```
IP1 master
IP2 worker
IP3 worker
```

### **Configure SSH**

You need to install open ssh on each node and you need to configure a passwordless ssh connection between nodes.

### **Installing and Confirguring Apache Spark**

We will do these steps on all nodes !!!

Download Apache Spark

```
wget https://dlcdn.apache.org/spark/spark-3.2.1/spark-3.2.2-bin-hadoop2.7.tgz
```

Extract the file

```
tar xvf spark-3.2.2-bin-hadoop2.7.tgz
```

Open .bashrc file and ddd following lines into .bashrc file

```
export JAVA_HOME=/home/spark/zulujdk11
export HADOOP_HOME=/home/spark/hadoop-2.7.7
export PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$PATH
export PATH=$PATH:/home/spark/spark-3.2.2-bin-hadoop2.7/bin
export SPARK_HOME=/home/spark/spark-3.2.2-bin-hadoop2.7
export SPARK_DAEMON_MEMORY=8g
export PYSPARK_PYTHON=/usr/bin/python3
```

Create spark-env.sh file by copying spark-env.sh.template and open this file 

```
cd /home/spark/spark-3.2.2-bin-hadoop2.7/conf

cp hadoopconfig.xml files into conf directory
cp spark-env.sh.template spark.env.sh

vim spark.env.sh
```

Add the following lines into spark-env.sh file 

```
export SPARK_WORKER_CORES=32 #, to set the number of cores to use on this machine
export SPARK_WORKER_MEMORY=100g #, to set how much total memory workers have to give executors (e.g. 1000m, 2g)
export SPARK_WORKER_WEBUI_PORT=7090
export SPARK_WORKER_PORT=40774
export SPARK_MASTER_WEBUI_PORT=30000
export SPARK_WORKER_OPTS="-Dspark.worker.cleanup.enabled=true -Dspark.worker.cleanup.interval=7200 -Dspark.worker.cleanup.appDataTtl=259200"
export SPARK_EXECUTOR_CORES=4 #, Number of cores for the executors (Default: 1).
export SPARK_EXECUTOR_MEMORY=30G #, Memory per Executor (e.g. 1000M, 2G) (Default: 1G)
export JAVA_HOME=/data5/spark/zulujdk11
export HADOOP_HOME=/home/spark/hadoop-2.7.7
export SPARK_LOCAL_IP=IP #, to set the IP address Spark binds to on this node
export SPARK_LOCAL_DIRS=/dev/shm/spark,/run/spark, 
export SPARK_MASTER_HOST=IP
export PYSPARK_PYTHON=/usr/bin/python3
export PYSPARK_DRIVER_PYTHON=/usr/bin/python3
```
Add following content in spark-defaults.conf

```
spark.master  spark://IP:7077
spark.eventLog.enabled true
spark.eventLog.dir file:///data2/spark/eventlogs
spark.history.fs.logDirectory file:///data2/spark/eventlogs 
spark.history.ui.port 28080
spark.history.fs.cleaner.enabled true
spark.history.fs.cleaner.interval 1d
spark.history.fs.cleaner.maxAge 3d
spark.eventLog.rolling.enabled true
spark.eventLog.rolling.maxFileSize 128m
spark.eventLog.compress true
spark.eventLog.compression.codec snappy
```

Create a bash script for delgation token and kinit

```
export PATH=/home/spark/spark-3.2.2-bin-hadoop2.7/bin:$PATH
export JAVA_HOME=/home/spark/zulujdk11
kinit -kt /home/spark/keytabs/spark.keytab spark@REAL.com
rm -rf /SPARKLOCK/spark_standalone/spark/keytabs/spark.token
spark-submit --class org.apache.hadoop.hdfs.tools.DelegationTokenFetcher "" --renewer null spark/keytabs/spark.token
#hdfs fetchdt --renewer null /home/spark/keytabs/spark.token
find /dev/shm/spark2 -type d -mmin +360 \-exec rm -rf {} \;
find /tmp -type d -mtime +1 -exec rm -rf {} \;
```

### **Start Spark Cluster**

start Master on master node in $SPARK_HOME/sbin directory

```
./start-master.sh
```

start worker on worker node in $SPARK_HOME/sbin directory

```
./start-worker.sh spark://IP:7077
```

**Congratulations :)**

Your spark cluster is up and running !!!


