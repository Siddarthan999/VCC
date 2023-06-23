1. Create a dedicated user account for hadoop
```
$sudo addgroup hadoop
$sudo adduser --ingroup hadoop hduser
$sudo usermod -a -G sudo hduser
$su - hduser
```
2. Install java1.8 in folder /usr/local
```
$sudo chmod 777 /usr/local
$cd /usr/local
$sudo tar xvzf $HOME/Downloads/jdk1.8.tar.gz
```
3. Install Hadoop
```
$cd /usr/local
$tar xvzf $HOME/Downloads/hadoop-2.5.1.tar.gz
$sudo chmod 777 hadoop-2.5.1
```
4. Set the hadoop environment variables: Include the following lines in the
$HOME/.bashrc file
```
# Set Hadoop-related environment variables 
export HADOOP_HOME=/usr/local/hadoop-2.5.1
# Set JAVA home directory
export JAVA_HOME=/usr/local/jdk1.8.0_31
#Add Hadoop bin/ directory to PATH
export PATH=$PATH:$HADOOP_HOME/bin
```
5. Set hadoop environment variables: Include the following lines /etc/profile file
```
#--insert JAVA_HOME 
JAVA_HOME=/usr/local/jdk1.8.0_31 
#--insert HADOOP_PREFIX
HADOOP_PREFIX=/usr/local/hadoop-2.5.1
#--in PATH variable just append at the end of the line 
PATH=$PATH: $JAVA_HOME/bin:$HADOOP_PREFIX/bin
#--Append HADOOP_PREFIX at end of the export statement 
export PATH JAVA_HOME HADOOP_PREFIX
```
6. Run the.bashrc & profile files from the $ prompt for updating the changes
```
$ source $HOME/.bashrc
$ source /etc/profile
```
Verify java & hadoop installation using
```
$ java -version
$ echo $HADOOP_PREFIX
$ cd $HADOOP_PREFIX
$ bin/hadoop version
```
7. Configuration of the hadoop files: hadoop-env.sh, core-site.xml, mapred-site.xml, hdfssite.xml and yarn-site.xml

path :: /usr/local/hadoop-2.5.1/etc/hadoop
a) hadoop-env.sh
Include the following lines in hadoop-env.sh file
```
export JAVA_HOME=/usr/local/jdk1.8.0_31
export HADOOP_PREFIX=/usr/local/hadoop-2.5.1
```
b) core-site.xml
Configure the directory for Hadoop to store its data files, the network ports it listens to, etc. Setup will use Hadoop’s Distributed File System(HDFS-single local machine)
```
$ mkdir -p /app/hadoop/tmp
$ chown hduser:hadoop /app/hadoop/tmp
```
Include the following lines in core-site.xml file between <configuration> and </configuration> tags
```
<property>
<name>hadoop.tmp.dir</name>
<value>/app/hadoop/tmp</value>
</property>
<property>
<name>fs.default.name</name>
<value>hdfs://localhost:9000</value>
</property>
```
c) mapred-site.xml
```
$sudo cp mapred-site.xml.template mapred-site.xml
```
Include the following lines in mapred-site.xml file
```
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>
```
d) hdfs-site.xml
Include the following lines in hdfs-site.xml file
```
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
```
e) yarn-site.xml
Include the following lines in yarn-site.xml file
```
<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
```
8. Format the Hadoop File system implemented on top of the local file system using
```
$/usr/local/hadoop-2.5.1/bin/hadoop namenode –format
```
9. Start Hadoop using
```
$sbin/start-all.sh
```
Explore Hadoop using http://localhost:50070/ from the browser

10. The commonly used HDFS Commands are as follows:
```
mkdir :
Creates a directory in the given path:
syntax: bin/hdfs dfs -mkdir <paths>
ls
Lists the files in a given path 
syntax:bin/hdfs dfs -ls <args> 
cp
Copies files from source to destination. This command allows 
multiple sources as well in which case the destination must be a 
directory.
syntax:bin/hdfs dfs -cp <source> <dest>
```
11.Create a directory ‘/input’ in HDFS
```
$bin/hdfs dfs -mkdir /input
```
12.Copy the input files into the distributed file system
```
$cd $HOME/Downloads/
$ tar xvzf $HOME/Downloads/mrsampledata.tar.gz
```
13.Run some of the examples provided
```
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduceexamples-2.5.1.jar grep /input /output '(CSE)'
```
14. Examine the output files
```
$ cd $HADOOP_PREFIX
$bin/hdfs dfs -put $HOME/Downloads/mrsampledata/* /input
```
Copy the output files from the distributed file system to the local file system and 
examine them:
```
$ bin/hdfs dfs -get output output
$ cat output/*
```
or
View the output files on the distributed file system
```
$ bin/hdfs dfs -cat /output/*
```
