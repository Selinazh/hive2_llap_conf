<snippet>
  <content>
Run Hive LLAP on Mac OS dev environment
=============

## Build Hive master 
Check out master: `git clone https://github.com/apache/hive.git`


`mvn clean package eclipse:clean eclipse:eclipse -Pitests -DskipTests -Pdist`


## Build Slider master
SLIDER-942 fixes SSL certificate issue. However, it is only available from Slider 1.0.0.

`mvn clean site:site site:stage package -DskipTests`

Use the generated compressed tar file in slider-assembly/target directory,untar to /usr/local/Cellar/slider

Configure Slider

- Append JAVA_HOME and HADOOP_CONF_DIR to conf/slider-env.sh

- Create sym link: `sudo ln -s /usr/local/Cellar/slider/slider-SNAPSHOT-incubating/bin/slider /usr/local/bin/slider` 

## Brew install zookeeper and start zookeeper server

## Download tez distribution (tez 0.8.4 and above)

- Copy tez-0.9.0-SNAPSHOT-minimal.tar.gz to hdfs direcoty /Users/${user.name}/apps/, copy tez-site.xml to hive distribution/conf

- Copy hive-env.sh to hive distribution/conf, modify local TEZ_HOME

## Copy hive-site.xml to hive distribution

## Copy RunLlapDaemon.sh to hive distribution/scripts/llap/bin

- Change HADOOP_CLASSPATH (copy output `hadoop classpath`)

## Run slider_gen.sh

- Start slider and llap daemon using generated llap-slider-date/run.sh

- Stop llap0 (default cluster name `slider stop llap0`) and kill slider application `yarn application -kill xxx`(leave llap daemon running)

- Now Hive CLI can run on llap mode

## References:

http://eastcirclek.blogspot.com/2016/10/how-to-start-hive-llap-functionality.html?view=flipcard&m=1

https://github.com/t3rmin4t0r/tez-autobuild

llap daemon runs on default port 15551, can be checked at 

`lsof -i:15551`

  </content>
</snippet>
