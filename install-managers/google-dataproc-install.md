# Installing Hadoop on Dataproc

Instructor:


```bash
gcloud beta dataproc clusters create tim-dataproc-test1 --enable-component-gateway --region us-central1 --subnet default \
--zone us-central1-a --master-machine-type n1-standard-4 --master-boot-disk-size 1000 --num-workers 5 \
--worker-machine-type n1-standard-4 --worker-boot-disk-size 1000 --image-version 1.3-deb9 \
--optional-components ANACONDA,HIVE_WEBHCAT,JUPYTER,ZEPPELIN,DRUID,PRESTO,ZOOKEEPER \
--initialization-actions gs://elephantscale-dataproc/dataproc-initialization-actions/hbase/hbase.sh \
--initialization-actions gs://elephantscale-dataproc/dataproc-initialization-actions/hue/hue.sh \
--tags ubuntu --project hadoop-setup-245603


gcloud beta compute --project=hadoop-setup-245603 instances create tim-bastion --zone=us-central1-a --machine-type=n1-standard-2 \
--subnet=default --network-tier=PREMIUM --can-ip-forward --maintenance-policy=MIGRATE \
--service-account=820551172242-compute@developer.gserviceaccount.com \
--scopes=https://www.googleapis.com/auth/cloud-platform --tags=ubuntu,public-open,http-server,https-server \
--image=ubuntu-training1 --image-project=hadoop-setup-245603 --boot-disk-size=100GB --boot-disk-type=pd-standard \
--boot-disk-device-name=tim-bastion --reservation-affinity=any


gcloud beta compute --project "hadoop-setup-245603" ssh --zone "us-central1-a" "tim-dataproc-test-m"
```


## STEP 4) Post-instal
login to namenode

Setup the home hdfs directory

```bash
    sudo -u hdfs  hdfs  dfs -mkdir /user/$USER
    sudo -u hdfs  hdfs  dfs -chown $USER  /user/$USER
```
