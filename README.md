# llap_conf
How to Run llap on Mac OS

1. check out Hive master 


2. compile and eclipse enable 
mvn clean package eclipse:clean eclipse:eclipse -Pitests -DskipTests -DdownloadSources -DdownloadJavadocs -Pdist


3. brew install zookeeper and start zookeeper server
zkServer start 


4. Download slider 0.8.0 and create sym link in /usr/local/bin
  527  cp ~/Downloads/slider-assembly-0.80.0-incubating-all.tar.gz .
  528  tar -xvf slider-assembly-0.80.0-incubating-all.tar.gz
  530  cd slider-0.80.0-incubating/ 
  append JAVA_HOME and HADOOP_CONF_DIR to conf/slider-env.sh
  545  sudo ln -s /usr/local/Cellar/slider/slider-0.80.0-incubating/bin/slider /usr/local/bin/slider

(hit SSL certificate problem in slider 0.80, re-compile and apply slider-942)

5. compile tez-0.9.0-SNAPSHOT
need push tez-0.9.0-SNAPSHOT-minimal.tar.gz to hdfs, add these two lines to tez-site.xml

<property>
    <name>tez.lib.uris</name>
    <value>/Users/selinaz/apps/tez-0.9.0-SNAPSHOT-minimal/tez-0.9.0-SNAPSHOT-minimal.tar.gz</value>
  </property>

<property>
   <name>tez.use.cluster.hadoop-libs</name>
   <value>true</value>
</property> 

In hive-env.sh, need to specify the tez_home from local machine. 

6. Copy the above files to dist_directory/conf, mysql driver to dist_directory/lib
Need to set hive.llap.io.allocate.direct=false in hive-site.xml for no native support for ZLIB
Manually add HADOOP_CLASSPATH to runLlapDaemon.sh script

7. run dist_directory/conf/slider_gen.sh
dist_directory bin/hive --service llap --instances 1 --args "-XX:+UseG1GC -XX:TLABSize=64m -XX:+ResizeTLAB -XX:+UseNUMA -XX:+AggressiveOpts -XX:MetaspaceSize=1024m -XX:InitiatingHeapOccupancyPercent=80 -XX:MaxGCPauseMillis=200 -XX:+PreserveFramePointer" 

7. Stop slider (not work for now, need investigate)
yarn application -kill to stop slider application master
slider stop llap0 to stop llap0 cluster

8. LlapDaemon is till running. Able to take query from Hive CLI. 

Troubleshoot:

LlapDaemon runs on 15551 port. For Mac OS does not have -p for netstat, using 
lsof -i :15551  

Useful note:
http://eastcirclek.blogspot.com/2016/10/how-to-start-hive-llap-functionality.html?view=flipcard&m=1

Goal’s repository
https://github.com/t3rmin4t0r/tez-autobuild
