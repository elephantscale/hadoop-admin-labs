### Hadoop Administration - multi-node install
Hadoop 1

Lab goal : install and configure a hadoop cluster

Be sure to complete 'first-steps.txt'



-- step )
intall JDK on all nodes
    $  ~/scripts/cluster-cmd.sh -h ~/hosts.all   "cd hi/software/java; sudo  sh ./jdk-6u45-linux-x64-rpm.bin"



-- step )
verify java is installed on all nodes
    $  ~/scripts/cluster-cmd.sh  -h ~/hosts.all   java -version
we should see the following output from all nodes!
    java version "1.6.0_45"
    Java(TM) SE Runtime Environment (build 1.6.0_45-b06)
    Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01, mixed mode)



-- step )
set up hadoop data dir : /hadoop/hadoop-data
    $  ~/scripts/cluster-cmd.sh  -h ~/hosts.all  "sudo mkdir -p /hadoop/hadoop-data"


-- step )
set up hdfs dirs

    $  ~/scripts/cluster-cmd.sh  -h ~/hosts.all  "sudo mkdir -p /hadoop/hadoop-data/dfs/name  /hadoop/hadoop-data/dfs/data   /hadoop/hadoop-data/namesecondary"

    $  ~/scripts/cluster-cmd.sh  -h ~/hosts.all  "sudo chown -R hdfs:hadoop /hadoop/hadoop-data/dfs"


-- step )
set up mapred dirs
    $  ~/scripts/cluster-cmd.sh  -h ~/hosts.all  "sudo mkdir -p /hadoop/hadoop-data/mapred/local"
    $  ~/scripts/cluster-cmd.sh  -h ~/hosts.all  "sudo chown -R mapred:hadoop /hadoop/hadoop-data/mapred"


-- step )
configure hadoop
sample config files are provided in :   ~/hi/hadoop-labs/hadoop-admin/config/multi-node
Let us copy these files into hadoop config dir  /etc/hadoop
    $   sudo  cp   ~/hi/hadoop-labs/hadoop-admin/config/multi-node/*   /etc/hadoop/

we need to edit two files to match your system
    $   sudo   vi /etc/hadoop/core-site.xml

look for the property
        <name>fs.default.name</name>
        <value>hdfs://namenode_hostname:8020</value>
we need to update the string 'namenode_hostname' to match your master's IP address.

on your node issue the command   'hostname'   to find out your ip
    $ hostname
output might be
    ip-10-154-179-237
this is the hostname of the master,  let's plug this value in to <value> tag

so the fs.default.name  will look like
        <name>fs.default.name</name>
        <value>hdfs://ip-10-154-179-237:8020</value>


Similary modify   /etc/hadoop/mapred-site.xml
    $ sudo   vi /etc/hadoop/mapred-site.xml

change the 'jobtracker_hostname' text to the ip address of your master
    <property>
        <name>mapred.job.tracker</name>
        <value>jobtracker_hostname:8021</value>
    </property>

e.g
it might look like:
    <property>
        <name>mapred.job.tracker</name>
        <value>ip-10-154-179-237:8021</value>
    </property>



-- step )
distribute the config files.
    $ ~/scripts/distribute-config.sh    -h   ~/hosts.slave


step )
verify config files are updated on slaves
login to one slave and inspect the file  /etc/hadoop/core-site.xml
    $ ssh <ip address of slave here>
once logged in
    $  cat  /etc/hadoop/core-site.xml
verify the ip address has been udpated
    $ logout



-- step )
format namenode
    $  sudo -u hdfs hadoop namenode -format

verify no errors and the output says...
    Storage directory /hadoop/hadoop-data/dfs/name has been successfully formatted.


-- step )
start namenode on master
    $  sudo service hadoop-namenode start

-- step)
[optional] start secondary namenode
    $  sudo service hadoop-secondarynamenode start


-- step )
verify namenode is running

verify 1)
namenode logs will go to '/var/log/hadoop/'  dir.  Inspect the log to make sure there are no errors
    $   less /var/log/hadoop/hdfs/*namenode*

verify 2)
we can also use JPS command to verify namemnode is running
    $ sudo /usr/java/latest/bin/jps

(we need to use sudo jps command, because namenode is running as user 'hdfs' not as user 'ec2-user')

The output might look something like this
    1324 NameNode
    1849 SecondaryNameNode
    1895 Jps

great, namenode processes are running


verify 3) using namenode web UI
go to this UI in your browser
    http://master_host_name:50070

we see namenode is running.

Question : any data nodes?
Question : what is the HDFS capacity?


-- step )
let's start datanodes on workers (not on master)
    $  ~/scripts/cluster-cmd.sh -h ~/hosts.slaves  "sudo service hadoop-datanode start"


-- step )
verify datanodes are running.

method 1)
refresh namenode UI again.  We should see datanodes connected and HDFS capacity increasing!

If a datanode is missing, trouble shoot it by logging into that node and inspecting the log file
    datanode logs go to  /var/log/hadoop/  dir

method 2)
using command line
    $   sudo -u hdfs  hadoop dfsadmin -report


-- step)
'kick the tires for hdfs'

let's create a 'home' dir in HDFS for 'ec2-user'
    $  sudo -u hdfs hadoop dfs -mkdir /user/ec2-user
    $  sudo -u hdfs hadoop dfs -chown ec2-user /user/ec2-user

let's copy a file as ec2-user
    $  hadoop dfs -put README.txt  .
(don't forget the dot at the end!)

verify if the file is there:
    $  hadoop dfs -ls

output will look similar to the following
    -rw-r--r--   2 ec2-user supergroup          6 2013-06-19 22:10 /user/ec2-user/README.txt


Excellent!  we have HDFS is running.


now let's start Map Reduce

-- step )
setup mapreduce directories in hdfs

    $  sudo -u hdfs  hadoop dfs -mkdir /mapred/system
    $  sudo -u hdfs  hadoop dfs -chown -R mapred /mapred
    $  sudo -u hdfs  hadoop dfs -chmod -R 700 /mapred

    $  sudo -u hdfs  hadoop dfs -mkdir /tmp
    $  sudo -u hdfs  hadoop dfs -chmod 777 /tmp

    $  sudo -u hdfs  hadoop dfs -mkdir /user/history
    $  sudo -u hdfs  hadoop dfs -chmod 777 /user
    $  sudo -u hdfs  hadoop dfs -chmod 777 /user/history


-- step )
start job tracker on master node
    $  sudo service hadoop-jobtracker start


-- step )
verify job tracker is running

method 1)
go to url  http://master_host_name:50030
if you see mapreduce admin dashboard then job tracker is running!

method 2)
    $  sudo /usr/java/latest/bin/jps

output might look like...
    2956 JobTracker   
    3101 Jps
    1324 NameNode
    1849 SecondaryNameNode


method 3)
inspect log file
    $  less /var/log/hadoop/mapred/hadoop-mapred-jobtracker*.log


-- step)
start task tracker on worker nodes
    $   ~/scripts/cluster-cmd.sh -h ~/hosts.slaves   "sudo service hadoop-tasktracker start"


-- step)
verify task trackers are up and can connect to job tracker

on the job tracker UI, check 'Nodes' section.  Do we see all task trackers connected?


It has been a long lab, but at the end we should have a working hadoop cluster.

Next Steps:
    look under ../verify-setup lab.
    Run a benchmark (e.g. TestDFSIO) to verify everything is working
