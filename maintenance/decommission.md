# How do we decommission a node gracefully

(to upgrade the machine, to update software ..etc)

* Command-line

### STEP 1)
    Find the file that is pointed by 'dfs.exclude.hosts' property
usually   /etc/hadoop/conf/dfs.excludes


### STEP 2)
    Add en entry for the host in that file (one line per host)
    e.g.
        dn12.hadoop1.mycompany.com

### STEP 3) refresh configuration
    $ hdfs dfsadmin  -refreshNodes


### STEP 4)
    check web UI to make sure the host is decommissioned
    also can use command line
        $ hdfs dfsadmin -report

### STEP 5)
    shutdown the machine and do any needed maitantance


### STEP 6)
    bring the machien back up

### STEP 7)
    remove the machine entry from 'dfs.exclude.hosts' file

### STEP 8)
    refreshNodes

### STEP 9)
    check to make the node has joined the cluster
    UI & dfsadmin -report

* Easier - using CM or Ambari

