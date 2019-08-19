# This lab teaches the use of Cloudera Manager (CM)

(For instructor)
- UI guide is in : /hadoop-training/doc/Installing_Cloudera_Cluster_on_EC2_Public.docx.
- hosts given to students should be launched in the "hadoop" security group, same as CM

#### STEP 1)  Login to the instance
Recommended instance types
    AMI : ubuntu 16.06 - ami-0f9cf087c1f27d9b1

    type : memory 64G recommended -
        r2.xl = 30 G
        r4.2xl = 64G memory

 - pick one machine for Cloudera Manager.
 - login with key
 ```
    ssh -i  hi1.pem   ubuntu@host_name
```

(On ubuntu $USER = ubuntu,  On CentOS $USER = ec2-user)

#### STEP 2) Installing Cloudera Manager (installing the installer)

CDH6
```bash
    wget https://archive.cloudera.com/cm6/6.3.0/cloudera-manager-installer.bin    
    chmod u+x cloudera-manager-installer.bin
    sudo ./cloudera-manager-installer.bin
```

Note: setting swappiness below is not required if you are using ES AMI hadoop-clean-V16 or later

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
* Provide **internal IPs** of the servers that will go into the cluster
* Finish up the install with the wizard


### Step 5: Configure student profile in CM
- Administration --> New User
- user name : student,   password : what ever
- role : readonly
- this is the credential we give out to students

#### STEP 6) Create a home directory in HDFS
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


#### Step 7 : Enable password less SSH on one client machine
Only need to do it on ONE machine.  
So students can login easily without the need for key.

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
