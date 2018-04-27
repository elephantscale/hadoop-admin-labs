# Backing up hadoop file system

distcp command can be used to backup one hdfs file system to another
it works like rsync (sending updates / diffs across)

Usage:

     $ hadoop distcp

e.g. 
 
      $ hadoop distcp  -p -update  "hdfs://namenode_1/important_data"   "hdfs://namenode_2/backup"
