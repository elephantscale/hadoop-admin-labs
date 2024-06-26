# This lab teaches the use of Cloudera Manager (CM)


#### STEP 1)  Login to the instance
Recommended instance types
    AMI: ubuntu 18.04 LTS
    instance type :
        - i3.xlarge (4 vcpu + 30G mem + ~1TB SSD + High network ) -- 31c / hr
        - m2.xlarge (4 vcpu + 30G mem + 850 G SSD + Moderate network) -- 49c / hr


 - pick one machine for Cloudera Manager.
 - login with key
 ```
    ssh -i  hi1.pem   ubuntu@host_name
```

(On ubuntu $USER = ubuntu,  On CentOS $USER = ec2-user)

#### STEP 2) Installing Cloudera Manager (installing the installer)

CDH6
```bash
    #wget https://archive.cloudera.com/cm6/6.3.0/cloudera-manager-installer.bin      # v6.3
    wget https://archive.cloudera.com/cm6/6.3.1/cloudera-manager-installer.bin    
    #wget http://archive.cloudera.com/cm7/7.1.3/cloudera-manager-installer.bin      # v7.1.3 if you want cdh7
    chmod u+x cloudera-manager-installer.bin
    sudo ./cloudera-manager-installer.bin
```
Note: Using version 6.3.1 will also work, 6.3.2 link is broken, and 6.3.3. requires a subscription
Note: Version 7.1.3 does appear to allow users to install a trial edition

Note: setting swappiness below is not very important

Set swappiness to 0 on every node, like this:

```bash
    sudo bash -c "echo 'vm.swappiness = 0' >> /etc/sysctl.conf" - then reboot
    for immediate change do 'sudo sysctl vm.swappiness=0'
```

#### STEP 3) Install CM
* Follow the prompts of CM installer on the terminal screen.  
* Once the installer is done, it will ask you to login in the browser, to finish the install
* In your browser to go  your_external_host_name:7180

#### STEP 4) Install the cluster using CM

* In the browser, login with admin/admin.
* change the password immediately
* Select Standard License
* Provide **internal Hostnames** of the servers that will go into the cluster
* Finish up the install with the wizard


#### STEP 5) Create a home directory in HDFS
- Login to 'client' node
- setup HDFS directories as follows
```bash
    sudo -u hdfs   hdfs dfs -mkdir   /user/$USER
    sudo -u hdfs  hdfs dfs -chown $USER /user/$USER
    hdfs dfs -mkdir  /user/$USER/sujee
```

#### STEP 6) Explore the cluster

* Click on HDFS service,  Check out Namenode WebUI
* Click on 'Hue' service.  Access Hue Web UI
* Explore File Manager / Job Manager

- Login to the node via ssh (using key)
- set the password to ubuntu user as follows
```
    $  sudo passwd ubuntu
    # choose a password
```
- edit this File
```
    $  sudo vi /etc/ssh/sshd_config
```
- Find the line
```
    PasswordAuthentication no
```
- change  it to
```
    PasswordAuthentication yes
```
-  reload ssh
```
    $  sudo /etc/init.d/ssh restart
    $ sudo service ssh reload
```
- try to remote login to the machine without ssh key and with password


#### STEP 7) Explore the cluster

* Close the hadoop-labs repository to the CM node
* Run the script to create data for HIVE

### Optional steps
Note that this is not required if you are using ES AMI hadoop-clean-V16 or later.

For cluster health, set ntp on every node.

```bash
	sudo yum -y install ntp
	sudo chkconfig ntpd on
	sudo ntpdate 0.centos.pool.ntp.org
	sudo service ntpd start


	set swappiness to 0
    # Note that this is not required if you are using hadoop-clean-V16
    sudo bash -c "echo 'vm.swappiness = 0' >> /etc/sysctl.conf"
    sudo sysctl vm.swappiness=0
```
