# Fair Scheduler

This lab allows you to experiment with the behaviour of Fair Scheduler   

### STEP 1)  Open the scheduler UI
 
    http://<resource-manager>:8088/cluster/scheduler
  
### STEP 2) Prepare to run multiple jobs in separate monitor windows

* For example, you can use the Hadoop example calculating the number PI, since it is long-running

* Give each job a different name, using the -D option

      mapred -D mapreduce.job.name = <yourname>-job1
      
* Submit the jobs to different queues, using -D mapreduce.job.queuename = <yourname>-queue1 (for example)

### STEP 3) Observe the resuls of the changes in Fair Scheduler on the running of the jobs

* Observe the jobs running in different pools, try to manipulate job execution by changing parameters 
* Be mindful of other students on the same cluster)

