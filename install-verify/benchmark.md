=====================
Benchmark : TestDFSIO
=====================
TESTDFSIO  is a benchmark that writes / reads files  to/from HDFS.
It uses mapreduce to do this in parallel.
This benchmark is part of Hadoop; it is pretty easy to run.

(HH = hadoop_home  dir)

Hadoop version 1 :
-----------------

    [Single Node]
        write test
            $ hadoop jar HH/hadoop-test*.jar TestDFSIO -write -nrFiles 5 -fileSize 500

        read test
            $ hadoop jar HH/hadoop-test*.jar TestDFSIO -read -nrFiles 5 -fileSize 500


    [Multi Node]
    Things are a little bit different on multinode cluster

    $   cd /tmp
    $   sudo -u hdfs   hadoop  jar /usr/lib/hadoop/hadoop-test-*.jar   TestDFSIO  -write -nrFiles 5  -fileSize 100

    Notes:
        - run as user hdfs
        - we switch to /tmp dir so hdfs user can write out the summary file
        - hadoop-test*jar could be usually found in one of these dirs
            /usr/share/hadoop
            /usr/lib/hadoop

    Example: On Cloudera parcels install
    sudo -u hdfs hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-client-jobclient-2.0.0-cdh4.5.0-tests.jar TestDFSIO -write -nrFiles 10 -fileSize 1000


Hadoop version 2:
----------------
    $ cd  /tmp
    $ sudo -u hdfs hadoop org.apache.hadoop.fs.TestDFSIO  -write -nrFiles 5  -fileSize 1GB


    (obsolte
    $ hadoop jar /usr/lib/hadoop-0.20-mapreduce/hadoop-test.jar  TestDFSIO -write -nrFiles 10 -fileSize 1GB
    )

Interpretting results:
----
At the end of run TestDFSIO will print out stats like the following
     fs.TestDFSIO: ----- TestDFSIO ----- : write
     fs.TestDFSIO:            Date & time: Mon Jun 17 22:08:29 UTC 2013
     fs.TestDFSIO:        Number of files: 10
     fs.TestDFSIO: Total MBytes processed: 1000
     fs.TestDFSIO:      Throughput mb/sec: 15.201726916177678
     fs.TestDFSIO: Average IO rate mb/sec: 16.754920959472656
     fs.TestDFSIO:  IO rate std deviation: 6.348449064660557
     fs.TestDFSIO:     Test exec time sec: 109.847


also a corresponding  -read option


=====================
Benchmark : Terasort
=====================
sort 1TB of random data
    10 billion records (10billion x 100 bytes each = 1000 billion = 1TB)

hadoop 2:
generating 10G of data:
    $  hadoop jar /usr/lib/hadoop-0.20-mapreduce/hadoop-examples.jar   teragen -D mapred.map.tasks=20  -D dfs.block.size=536870912  100000000   tera/in
    
The exact jar location depends upon the distribution and version. For example, in HDP
/usr/hdp/2.6.2.14-5/hadoop-mapreduce/hadoop-mapreduce-examples.jar    

- various inputs
        10 000 000 000  = 10 Billion records = x 100 bytes = 1 TB
        1 000 000 000  = 1 Billion records = x 100 bytes = 100 GB
        100 000 000  = 100 million records = x 100 bytes = 10 GB

- we are using block size 512MB
If you have a very fast cluster, your map tasks might finish in a few seconds if you use a relatively small default HDFS block size such as 128MB (which is not a bad idea though in general). This means that the time to start/stop map tasks might be larger than the time to perform the actual task. In other words, the overhead for managing the TaskTrackers might exceed the job's "payload". An easy way to work around this is to increase the HDFS block size for files written by TeraGen. Keep in mind that the HDFS block size is a per-file setting, and the value specified by the 'dfs.block.size'

Sorting the data:
    $ hadoop jar /usr/lib/hadoop-0.20-mapreduce/hadoop-examples.jar  terasort -D mapred.reduce.tasks=20   tera/in   tera/out
