#Installing Horton Works on Amazon

Instructor:

* spin up our Hadoop image
* instances :
    - i3.xlarge (4 vcpu + 30G mem + ~1TB SSD + High network ) -- 31c / hr
    - m2.xlarge (4 vcpu + 30G mem + 850 G SSD + Moderate network) -- 49c / hr

## Summary

On the master

    sudo wget -O /etc/apt/sources.list.d/ambari.list http://public-repo-1.hortonworks.com/ambari/ubuntu16/2.x/updates/2.6.1.5/ambari.list
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com B9733A7A07513CAD
    sudo apt update
    sudo apt-get install -y ambari-server
    sudo ambari-server setup 
    sudo ambari-server start
    
On each server, add
    
    sudo wget -O /etc/apt/sources.list.d/ambari.list http://public-repo-1.hortonworks.com/ambari/ubuntu16/2.x/updates/2.6.1.5/ambari.list
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com B9733A7A07513CAD
    sudo apt update
    sudo apt install ambari-agent

### NOTE

Lately, all Ambari installs appear broken on our CentOS images. You may have more success with Ubuntu.
In any case, plan ahead and leave more time for install.

Ambari 2.6 has issues installing on Ubuntu machine.  It is not installing ambari-agent on hosts automatically using SSH auto login.  You have to install agents manually on each host.  Really annoying.
     https://community.hortonworks.com/questions/16281/install-ambari-agent-on-each-hosts.html
See below for steps

### STEP 1)
Login to one machine instance to install Ambari

Ambari 2.6.2
    https://cwiki.apache.org/confluence/display/AMBARI/Installation+Guide+for+Ambari+2.6.2



### STEP 2)
Execute these steps in terminal (hint: always accept all defaults :)

#### Ubuntu 16.04
```bash
    sudo wget -O /etc/apt/sources.list.d/ambari.list  http://public-repo-1.hortonworks.com/ambari/ubuntu16/2.x/updates/2.6.2.0/ambari.list

    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com B9733A7A07513CAD

    sudo apt-get update

    sudo apt-get install -y  ambari-server

    sudo ambari-server setup
        # keep hitting enter to accept default values

    sudo ambari-server start

    sudo ambari-server status

```

#### Centos 7 or RHEL 7

Set up the repo (notice the version)

    sudo wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.6.2.0/ambari.repo -O /etc/yum.repos.d/ambari.repo

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
  * user name: ec2-user in Centos ,   ubuntu in Ubuntu
* registering hosts fail!
    This is an annoying bug on Ambari + Ubuntu (may be others).
    Ambari Agent fails to install in remote machines.  
    Here is how you do it manually
        for each machine to install hadoop
        do
            # ssh to machine

            sudo wget -O /etc/apt/sources.list.d/ambari.list  http://public-repo-1.hortonworks.com/ambari/ubuntu16/2.x/updates/2.6.2.0/ambari.list

            sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com B9733A7A07513CAD

            sudo apt-get update

            sudo apt-get install -y ambari-agent

            sudo vi /etc/ambari-agent/conf/ambari-agent.ini
                # set the ambari hostname correctly
                [server]
                hostname=a.b.c.d.internal

            sudo ambari-agent start

            sudo systemctl enable ambari-agent
        done

        Now hit 'Retry Failed'  in the UI it will succeed!

* for service allocation, accept the defaults
* install clients on all nodes
* password: admin for all services


## STEP 4) Post-instal
login to namenode

Setup the home hdfs directory

    $  sudo -u hdfs  hdfs  dfs -mkdir /user/$USER
    $  sudo -u hdfs  hdfs  dfs -chown $USER  /user/$USER
