# llap_conf
How to Run llap on Mac OS
<snippet>
<content>

1. check out Hive master 

2. compile and eclipse enable 
`
mvn clean package eclipse:clean eclipse:eclipse -Pitests -DskipTests -DdownloadSources -DdownloadJavadocs -Pdist
`

3. brew install zookeeper and start zookeeper server
`
zkServer start 
`

4. Download slider 0.8.0 and create sym link in /usr/local/bin
`  
cp ~/Downloads/slider-assembly-0.80.0-incubating-all.tar.gz .
  
tar -xvf slider-assembly-0.80.0-incubating-all.tar.gz
  
cd slider-0.80.0-incubating/
  
append JAVA_HOME and HADOOP_CONF_DIR to conf/slider-env.sh
  
sudo ln -s /usr/local/Cellar/slider/slider-0.80.0-incubating/bin/slider /usr/local/bin/slider
`
 
 (hit SSL certificate problem in slider 0.80, re-compile and apply slider-942)

5. compile tez-0.9.0-SNAPSHOT
   need push tez-0.9.0-SNAPSHOT-minimal.tar.gz to hdfs, add tez.lib.uris and tez.use.cluster.hadoop-libs to tez-site.xml
   In hive-env.sh, need to specify the tez_home from local machine.

6. Copy files in this repository to dist_directory/conf, mysql driver to dist_directory/lib
   Need to set hive.llap.io.allocate.direct=false in hive-site.xml for no native support for ZLIB
   Manually add HADOOP_CLASSPATH to runLlapDaemon.sh script

7. run distDirectory/conf/slider_gen.sh

`
distDirectory/bin/hive --service llap --instances 1 --args "-XX:+UseG1GC -XX:TLABSize=64m -XX:+ResizeTLAB -XX:+UseNUMA -XX:+AggressiveOpts -XX:MetaspaceSize=1024m -XX:InitiatingHeapOccupancyPercent=80 -XX:MaxGCPauseMillis=200 -XX:+PreserveFramePointer"
`

8. Stop slider (not work for now, need investigate)
`
yarn application -kill to stop slider application master
slider stop llap0 to stop llap0 cluster
`

9. LlapDaemon is till running. Able to take query from Hive CLI.


Troubleshoot:

LlapDaemon runs on 15551 port. For Mac OS does not have -p for netstat, use
lsof -i :15551

copy debug.sh to distDirectory/bin/ext to solve GC confliction issue when debug Hive client. 

Useful note:
http://eastcirclek.blogspot.com/2016/10/how-to-start-hive-llap-functionality.html?view=flipcard&m=1

Goal’s repository
https://github.com/t3rmin4t0r/tez-autobuild
<snippet>
<content>
