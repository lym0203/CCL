# OpenTSDB install
### root Connection
```sh
$ sudo passwd root //root password change
$ su root
```
### JAVA Setting
```sh
$ java -version
javaversion "1.8.0"
java(TM) SE Runtime Enviroment (build 1.8.0-b132)
java Hotspot(TM) client VM(build 25.0-b70, mixed mode)
```
When not confirmed the Java version, Please refer to the link below because it is not installed.
https://opentutorials.org/module/516/5558 (Install in the " arm " version)

##### profile Add 3 lines to the bottom line
```sh
$ nano /etc/profile

JAVA_HOME=/usr/
export JAVA_HOME
export PATH=$PATH:$JAVA_HOME/bin
```
##### Apply profile After adding
```sh
$ source /etc/profile
```
### hbase Install
```sh
$ cd /usr/local
$ mkdir data
$ cd data
$ wget http://www.apache.org/dist/hbase/stable/hbase-1.0.1.1-bin.tar.gz
$ tar xvfz hbase-1.0.1.1-bin.tar.gz
$ cd hbase-1.0.1.1
$ hbase_rootdir=${TMPDIR-'/usr/local/data'}/tsdhbase
$ iface=lo'uname | sed -n s/Darwin/0/p'
```
##### hbase-site.xml correction
```sh
$ nano conf/hbase-site.xml
```
configuration Add to between the tags
```sh
<configuration>
	<property>
		<name>hbase.rootdir</name>
		<value>file:///DIRECTORY/hbase</value>
	</property>
	<property>
		<name>hbase.zookp.property.eataDir</name>
		<value>/DRECTORY/zookeeper</value>
	</property>
</configuration>
```
##### hbase.sh Run
```sh
$ ./bin/start-hbase.sh
master running as process (process number). Stop it first
```
※When " hbase.sh " run, If this is notification because the setting of the 'java_home JDK' is not successful,
  Run a " profile " again ($ source /etc/profile)
  ### GnuPlot Install
  ```sh
$ cd /usr/local
$ apt-get install gcc
$　apt-get install libgd2-xpm-dev
$ wget http://sourceforge.net/projects/gnuplot/files/gnuplot/4.6.3/gnuplot-4.6.3.tar.gz
$ tar zxvf gnuplot-4.6.3.tar.gz
$ cd gnuplot-4.6.3
$ ./configure
$ make install  //It takes about 10 minutes to complete.
$ apt-get install gnuplot
$ apt-get install dh-autoreconf
```
### OpenTSDB Install
  ```sh
$ cd /usr/local
$ git clone git://github.com/OpenTSDB/opentsdb.git

$ cd opentsdb
$ ./build.sh  <- It takes about 10 minutes to complete.
$ env COMPRESSION=NONE HBASE_HOME=/usr/local/data/hbase-1.0.1.1 ./src/create_table.sh 
$ tsdtmp=${TMPDIR-'/usr/local/data'}/tsd
$ mkdir -p "$tsdtmp"
```
### Open TSDB Run
```sh
$ ./build/tsdb tsd --port=4242 --staticroot=build/staticroot --cachedir=/usr/local/data --auto-metric
```
### Tcollector Install
Execution of one terminal anymore
```sh
$ cd /usr/local
$ git clone git://github.com/OpenTSDB/tcollector.git
$ cd tcollector
```
##### 'startstop' Changing the file
```sh
$ nano startstop
```
Please comment cancel the portion of the '#TSD_HOST=dns.name.of.tsd' and Enter the Ip address.(4~5 line)
```sh
TSD_HOST=192.168.X.X
```
### Tcollector Run - Execution of one terminal anymore
```sh
$ ./startstop start
$ tail -f /var/log/tcollector.log
```
##### The connection with 'http://192.168.x.x:4242' in the web address bar
