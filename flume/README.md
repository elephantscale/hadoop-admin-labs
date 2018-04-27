# This lab teaches the use of Flume

To instructor:

* each student can install flume on his node in the cluster. Alternatively, they can work in groups, so that each group has a node assigned to it
* each student can work in his own copy of the directory, generating data under his or her name

### STEP 1)  Installing Flume
 
* On managed clusters (Cloudera or Hortonworks) flume-ng is already installed. Verify this by running

      flume-ng
    
 
* If Flume is not installed, execute the following command

      sudo yum install --assumeyes flume-ng


### STEP 2) Describe the agent in the Flume configuration file

* You can create a centrally-located configuration file, or you can create your local version, then point to it. 
The instructions below are for the centrally-located version

* Create a config file by copying from the template

      sudo cp /usr/lib/flume-ng/conf/flume-conf.properties.template /etc/hadoop/conf/flume-conf.properties
      sudo nano /etc/hadoop/conf/flume-conf.properties
      (or use vi)
      
* Add agent description
    * Give your agent the name 'myagent'
    * The agent should collect data from a file and deposit it in hdfs
    * Use the instructions given in the slides to create the agent and it's parameters

      
### STEP 3) Verify the Flume config file by start flume-ng and checking for errors

### STEP 4) Generate data in the access log

  Run the data generating script
  
    cd hadoop-admin-labs/data
    python gen-clickstream.py
    
* You will give you a number of log files: 0.log, 1.log, 2.log, and so on. Y
* Later, you will be able to append data to you main log files, such as cat 1.log >> <your-log-file>
  
### STEP 5) Start collector in second terminal window
  
  Point to your Flume configuration file
  
  Use flume-ng command, such as the one below

    flume-ng agent -n myagent -c conf -f conf/flume-conf.properties

== STEP 6) Add data to the source 

Use a command like this


      1.log >> <your-log-file>
      2.log >> <your-log-file>

### STEP 7) Verify the files in HDFS
  
  Use 
  
     hdfs dfs -ls <your-output-directory>
     
     
Bonus

### STEP 8) Buffer the data

Goal: achieve larger HDFS files sizes

Hint: try to change the parameters below

* Number of seconds to wait before rolling current file (in seconds)

agent.sinks.sink.hdfs.rollInterval=0

* File size to trigger roll, in bytes (256Mb)

agent.sinks.sink.hdfs.rollSize = 268435456

agent.sinks.sink.hdfs.rollCount = 0

