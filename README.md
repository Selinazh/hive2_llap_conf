<snippet>
  <content>
Run LLAP on MAC OS
=============

## Build Hive master 
Check out master: `git clone https://github.com/apache/hive.git`
## Build Slider master
SLIDER-942 fixes SSL certificate issue. However, it is only available from Slider 1.0.0. After build,

1. Append JAVA_HOME and HADOOP_CONF_DIR to conf/slider-env.sh

2. Create sym link: `sudo ln -s /usr/local/Cellar/slider/slider-SNAPSHOT-incubating/bin/slider /usr/local/bin/slider` 

3. Brew install zookeeper and start zookeeper server

## Download tez distribution (tez 0.8.4 and above)

1. Copy tez-0.9.0-SNAPSHOT-minimal.tar.gz to hdfs, see changes in tez-site.xml

2. Change hive-env.sh to specify local TEZ_HOME

## Copy hive-site.xml to hive distribution

## Copy RunLlapDaemon.sh to hive distribution/scripts/llap/bin

Change HADOOP_CLASSPATH (copy output `hadoop classpath`)

## Run slider_gen.sh

Start slider and llap daemon using generated llap-slider-date/run.sh

## Stop llap0 (default cluster name) and kill slider application (leave llap daemon running)

## References:

http://eastcirclek.blogspot.com/2016/10/how-to-start-hive-llap-functionality.html?view=flipcard&m=1

https://github.com/t3rmin4t0r/tez-autobuild
 
  </content>
</snippet>
