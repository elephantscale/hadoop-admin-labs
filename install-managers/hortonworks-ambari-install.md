#Installing Horton Works on Amazon

Instructor:

* spin up our Hadoop image
* t2.xlarge instances, or t2.2xlarge

### NOTE

Lately, all Ambari installs appear broken on our CentOS images. You may have more success with Ubuntu.
In any case, plan ahead and leave more time for install.

### STEP 1) 
Login to one machine instance to install Ambari


### STEP 2)
Execute these steps in terminal (hint: always accept all defaults :)

#### Centos 7 or RHEL 7

Set up the repo (notice the version)

    sudo wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.6.0.0/ambari.repo -O /etc/yum.repos.d/ambari.repo
	sudo yum install -y ambari-server
	sudo ambari-server setup
	sudo ambari-server start

Currently (10/29/2017) this version of Hadoop (2.5.2) fails on CentOS7.
To fix this, use the following command on every node in the cluster
```
sudo yum-config-manager --enable rhui-REGION-rhel-server-optional
```

#### Point your browser to ambari server's hostname and port 8080
	http://your_ambar_server:8080

#### Login using: 
Username = admin
Password = admin 


## STEP 3) install guide

* give a name to your cluster
* select HDP 2.x
* input host names (use private IPs) 
  * ssh key will be provided by instructor  (hi1.pem)
  * user name: ec2-user
* for service allocation, accept the defaults
* install clients on all nodes
* password: admin for all services


## STEP 4) Post-instal
login to namenode

Setup the home hdfs directory

    $  sudo -u hdfs  hdfs  dfs -mkdir /user/$USER
    $  sudo -u hdfs  hdfs  dfs -chown $USER  /user/$USER

