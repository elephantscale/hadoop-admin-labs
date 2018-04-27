# This lab teaches the use of Cloudera Manager (CM)

(For instructor)
- latest guide is in : /hadoop-training/doc/Installing_Cloudera_Cluster_on_EC2_Public.docx.
- hosts given to students should be launched in the "hadoop" security group, same as CM

#### STEP 1)  Login to the instance
 Use the instance provided by the instructor, log in using SSH
 e.g
    ssh ec2-user@your_host_name

#### STEP 2) Installing Cloudera Manager (installing the installer)

CDH5  
    
    wget http://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin
    chmod +x cloudera-manager-installer.bin
    sudo ./cloudera-manager-installer.bin

Note: setting swappiness below is not required if you are using ES AMI hadoop-clean-V16 or later

Set swappiness to 0 on every node, like this:

    sudo bash -c "echo 'vm.swappiness = 0' >> /etc/sysctl.conf" - then reboot
    for immediate change do 'sudo sysctl vm.swappiness=0'

#### STEP 3) Install CM

* Follow the prompts of CM installer on the terminal screen.  
* Once the installer is done, it will ask you to login in the browser, to finish the install
* In your browser to go  your_external_host_name:7180

#### STEP 4) Install the cluster using CM
    
* In the browser, login with admin/admin.

* Select Standard License

* Provide internal IPs of the servers that will go into the cluster

* Finish up the install with the wizard

#### STEP 5) Create a home directory in HDFS

    sudo -u hdfs   hdfs dfs -mkdir   /user/$USER
    sudo -u hdfs  hdfs dfs -chown $USER /user/$USER
    hdfs dfs -mkdir  /user/$USER/sujee

(On ubuntu $USER = ubuntu,  On CentOS $USER = ec2-user)

#### STEP 6) Explore the cluster

    * Click on HDFS service,  Check out Namenode WebUI
    * Click on 'Hue' service.  Access Hue Web UI
    * Explore File Manager / Job Manager

For cluster health, set ntp on every node

	sudo yum -y install ntp
	sudo chkconfig ntpd on
	sudo ntpdate 0.centos.pool.ntp.org
	sudo service ntpd start
	
	
	set swappiness to 0
    # Note that this is not required if you are using hadoop-clean-V16
    sudo bash -c "echo 'vm.swappiness = 0' >> /etc/sysctl.conf"
    sudo sysctl vm.swappiness=0
    
	Note that this is not required if you are using ES AMI hadoop-clean-V16 or later

