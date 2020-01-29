# This lab teaches the use of Sqoop

To instructor :
   
* each student can work in his own copy of the directory, generating data under his or her name
* each student can install sqoop on his node in the cluster.

### STEP 1)  Getting a MySQL to use
 
* See [common labs](https://github.com/elephantscale/common-labs/blob/master/README.md)

### STEP 2) Load the log data into the table

* Here is what is in the database now (see below)
* Login and verify this

```text  

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| training           |
+--------------------+
2 rows in set (0.05 sec)

MariaDB [(none)]> use training;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [training]> describe mylogs;
+--------------+-------------+------+-----+---------+-------+
| Field        | Type        | Null | Key | Default | Extra |
+--------------+-------------+------+-----+---------+-------+
| message_type | varchar(20) | YES  |     | NULL    |       |
| m1           | varchar(20) | YES  |     | NULL    |       |
| m2           | varchar(20) | YES  |     | NULL    |       |
| m3           | varchar(20) | YES  |     | NULL    |       |
| m4           | varchar(20) | YES  |     | NULL    |       |
| m5           | varchar(20) | YES  |     | NULL    |       |
| m6           | varchar(20) | YES  |     | NULL    |       |
| m7           | varchar(20) | YES  |     | NULL    |       |
| m8           | varchar(20) | YES  |     | NULL    |       |
+--------------+-------------+------+-----+---------+-------+
9 rows in set (0.05 sec)

```

### STEP 3) Installing sqoop (if not installed)

* sqoop should be installed, if not - 'sudo yum install --assumeyes sqoop'
  
* Provide the required RDBS library 
  
            sudo ln -s ~/HI-labs/hadoop-dev/lib/mysql-connector-java-5.1.31-bin.jar /var/lib/sqoop/mysql-connector-java.jar #give it mysql connector 
  
 * (adjust the path if needed and open the permissions if you have to): 

### STEP 4) Operate sqoop for lookups
  
              sqoop help
              sqoop list-databases --connect jdbc:mysql://localhost --username root --password <password> # if using password
              sqoop list-tables --connect jdbc:mysql://localhost/training --username root --password <password> # if using password
  
### STEP 5) Import the data

* Use the statement similar to 
 
 
            load data local infile '0.log' into table mylogs fields terminated by ' '
            enclosed by '"'
            lines terminated by '\n'
            (message_type, m1, m2, m3, m4, m5)

            sqoop import --connect jdbc:mysql://localhost/training --table mylogs --fields-terminated-by '\t'  --username root --password <password> # if using password

 * Example for SQLServer

            sqoop import --table <table-name> --target-dir test/import -m 1 --connect "jdbc:sqlserver://<ip>;databaseName=<name>" --username <username> --password <password>

### STEP 6) Verify that import worked
  
          hdfs dfs -ls mylogs
          hdfs dfs -tail mylogs/part-m-00000

# After logging in to mysql:
load data local infile '0.log' into table mylogs fields terminated by ' '
        enclosed by '"'
        lines terminated by '\n'
        (message_type, m1, m2, m3, m4, m5)
        
hdfs dfs -rm -r mylogs
sqoop import --connect jdbc:mysql://localhost/training --table mylogs --fields-terminated-by '\t'  --username root -m 1
hdfs dfs -cat  mylogs/* 

# Complete script for your reference

Complete script with Hortonworks

sqoop list-databases --connect jdbc:mysql://localhost --username root
sqoop list-tables --connect jdbc:mysql://localhost/training --username root
sudo mv mysql-connector-java-6.0.6.jar /usr/lib/sqoop/lib/      
hdfs dfs -rm -r mylogs
sqoop import --connect jdbc:mysql://localhost/training --table mylogs --fields-terminated-by '\t'  --username root -m 1
hdfs dfs -cat  mylogs/*
