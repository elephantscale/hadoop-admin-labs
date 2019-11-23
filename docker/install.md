# HDP Deployment


## Run Download Script


```bash
sh docker-deploy-hdp30.sh
```
Note: You only need to run script once. It will setup and start the sandbox for you, creating the sandbox docker container in the process if necessary.

Note: The decompressed folder has other scripts and folders. We will ignore those for now. They will be used later in advanced tutorials.

The script output will be similar to:

## Verify HDP Sandbox

Verify HDP sandbox was deployed successfully by issuing the command:

```bash
docker ps
```
You should see something like:

```bash
docker-ps-hdp-output
```

## Stop HDP Sandbox
When you want to stop/shutdown your HDP sandbox, run the following commands:

```bash
docker stop sandbox-hdp
docker stop sandbox-proxy
```

### Restart HDP Sandbox

When you want to re-start your sandbox, run the following commands:

```bash
docker start sandbox-hdp
docker start sandbox-proxy
```

## Remove HDP Sandbox
A container is an instance of the Sandbox image. You must stop container dependancies before removing it. Issue the following commands:

```bash
docker stop sandbox-hdp
docker stop sandbox-proxy
docker rm sandbox-hdp
docker rm sandbox-proxy
```

If you want to remove the HDP Sandbox image, issue the following command after stopping and removing the containers:

```bash
docker rmi hortonworks/sandbox-hdp:{release}
```

## HDF Deployment
### Deploy CDF Sandbox
#### Install/Deploy/Start CDF Sandbox

Download latest scripts Cloudera DataFlow (CDF) for Docker and decompress zip file.

In the decompressed folder, you will find shell script docker-deploy-.sh. From the command line, Linux / Mac / Windows(Git Bash), run the script:

```bash
cd /path/to/script
sh docker-deploy-{HDFversion}.sh
```
Note: You only need to run script once. It will setup and start the sandbox for you, creating the sandbox docker container in the process if necessary.

Note: The decompressed folder has other scripts and folders. We will ignore those for now. They will be used later in advanced tutorials.

The script output will be similar to:

docker-start-hdf-output

### Verify CDF Sandbox
Verify HDF sandbox was deployed successfully by issuing the command:

```bash
docker ps
```

You should see something like:

```console
docker-ps-hdf-output
```

## Stop CDF Sandbox
When you want to stop/shutdown your HDF sandbox, run the following commands:

```bash
docker stop sandbox-hdf
docker stop sandbox-proxy
```

## Restart CDF Sandbox

When you want to re-start your HDF sandbox, run the following commands:

```bash
docker start sandbox-hdf
docker start sandbox-proxy
```

## Remove CDF Sandbox
A container is an instance of the Sandbox image. You must stop container dependencies before removing it. Issue the following commands:

```bash
docker stop sandbox-hdf
docker stop sandbox-proxy
docker rm sandbox-hdf
docker rm sandbox-proxy
```

If you want to remove the CDF Sandbox image, issue the following command after stopping and removing the containers:

````bash
docker rmi hortonworks/sandbox-hdf:{release}
```

## Enable Connected Data Architecture (CDA) - Advanced Topic
#### Prerequisite:

 * A computer with minimum 22 GB of RAM dedicated to the virtual machine
 * Have already deployed the latest HDP/HDF sandbox
 * Update Docker settings to use minimum 16 GB (16384 MB)
 * Hortonworks Connected Data Architecture (CDA) allows you to play with both data-in-motion (CDF) and data-at-rest (HDP) sandboxes simultaneously.

### HDF (Data-In-Motion)

Data-In-Motion is the idea where data is being ingested from all sorts of different devices into a flow or stream. While the data is moving throughout this flow, components or as NiFi calls them “processors” are performing actions on the data to modify, transform, aggregate and route it. Data-In-Motion covers a lot of the preprocessing stage in building a Big Data Application. For instance, data preprocessing is where Data Engineers work with the raw data to format it into a better schema. Data Scientists will focus on analyzing and visualizing the data.

### HDP (Data-At-Rest)

Data-At-Rest is the idea where data is not moving and is stored in a database or robust datastore across a distributed data storage such as Hadoop Distributed File System (HDFS). Instead of sending the data to the queries, the queries are being sent to the data to find meaningful insights. At this stage data, data processing and analysis occurs in building a Big Data Application.

### Update Docker Memory
Select Docker -> Preferences... -> Advanced and set memory accordingly. Restart Docker.

docker-memory-settings

Run Script to Enable CDA
When you first deployed the sandbox, a suite of deployment scripts were downloaded - refer to Deploy HDP Sandbox as an example.

In the decompressed folder, you will find shell script enable-native-cda.sh. From the command line, Linux / Mac / Windows(Git Bash), run the script:

```bash
cd /path/to/script
sh enable-native-cda.sh
```

The script output will be similar to:

```console
docker-enable-cda-output
```

## Appendix A: Troubleshooting
### Drive not shared
docker-drive-not-shared

Docker needs write access to the drive where the docker-deploy-.sh is executed.

The easiest solution is to execute script from Downloads folder.

Otherwise, go to Docker Preferences/Settings -> File Sharing/Shared Drives -> Add/Select path/drive where deploy-scripts are located and try again.

### No space left on device
Potential Solution
Increase the size of base Docker for Mac VM image
Port Conflict
While running the deployment script, you may run into conflicting port issue(s) similar to:

docker-conflicting-port

In the picture about, we had a port conflict with 6001.

Go to the location where you saved the Docker deployment scripts - refer to Deploy HDP Sandbox as an example. You will notice a new directory sandbox was created.

Edit file sandbox/proxy/proxy-deploy.sh
Modify conflicting port (first in keypair). For example, 6001:6001 to 16001:6001
Save/Exit the File
Run bash script: bash sandbox/proxy/proxy-deploy.sh
Repeat steps for continued port conflicts
Verify sandbox was deployed successfully by issuing the command:

```bash
docker ps
```
