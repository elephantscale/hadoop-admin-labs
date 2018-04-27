### Hadoop Administration - multi Node install -- setting up
Hadoop 1


-- step 1)
Login into the linux node.  Instructor will provide credentials
This will be the master node


-- step 2)
populate hosts files.  we will use these with automated scripts
edit  ~/hosts.all  (e.g.   vi ~/hosts.all)
enter ip-addresses of all hadoop nodes, one at a time
e.g
    ip-10-157-40-61.ec2.internal
    ip-10-235-1-237.ec2.internal
    ip-10-158-93-30.ec2.internal
    ip-10-28-104-201.ec2.internal

similarly, create a file ~/hosts.slaves  and enter ips of only worker nodes


-- step 3)
verify remote commands scripts

    $ ~/scripts/cluster-cmd.sh   -h ~/hosts.all  ls

the output might look like

    ====== ip-10-157-40-61.ec2.internal ======
    dev
    hi
    HOST__HI_TRAINING_VM_1
    hosts.all
    hosts.workers

    ====== ip-10-235-1-237.ec2.internal ======
    dev
    hi
    HOST__HI_TRAINING_VM_1

We will use these handy scripts to do run commands on remote nodes.
(saves us time logging into each node and running commands)
