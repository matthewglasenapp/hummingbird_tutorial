# colibri_tutorial
Tutorial for Hummingbird Colibri 

The syntax used to navigate the Hummingbird cluster is mostly not Hummingbird-specific. Hummingbird uses the Slurm workload manager software. Slurm (Simple Linux Utility for Resource Management) is a job scheduler that automates the process of allocating resources (i.e., hardware) for users' computational tasks. 

Learn more about this at:
1. https://hummingbird.ucsc.edu/
2. https://login.scg.stanford.edu/tutorials/job_scripts/

## 1. Basic slurm commands 
Check the Hummingbird Queue to see what jobs are currently running. 

```
squeue
```

```
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            424894    128x24     EDTA kamaryan  R    1:46:03      1 hbnode-20
            424893    128x24     EDTA kamaryan  R    1:46:09      1 hbnode-19
            424892    128x24     EDTA kamaryan  R    1:46:18      1 hbnode-18
            424891    128x24     EDTA kamaryan  R    1:46:27      1 hbnode-15
            424895    128x24     EDTA kamaryan  R    1:45:57      1 hbnode-21
            407851    128x24 genotype mglasena  R 5-21:23:42      1 hbnode-17
            422068    128x24  l_s=0.1    ppopp  R 4-00:19:27      1 hbnode-16
            424403    128x24 estimate    jeqli  R   18:24:22      1 hbnode-09
            424419    128x24 estimate    jeqli  R   17:58:17      1 hbnode-11
            424418    128x24 estimate    jeqli  R   17:58:20      1 hbnode-08
            424630    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-18
            424631    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-06
            424632    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-07
            424633    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-13
            424634    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-12
            424635    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-12
            424636    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-14
            424637    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-14
            424638    128x24  qgm_job  aazaman  R    9:59:00      1 hbnode-15
            424629    128x24  qgm_job  aazaman  R    9:59:01      1 hbnode-10
          424791_1    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_2    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_3    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_4    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_5    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_6    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_7    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_8    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
          424791_9    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
         424791_10    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
         424791_11    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-10
         424791_12    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_13    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_14    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_15    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_16    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_17    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_18    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_19    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_20    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_21    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_22    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_23    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_24    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_25    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_26    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_27    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_28    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_29    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_30    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_31    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_32    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_33    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_34    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-13
         424791_35    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_36    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_37    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_38    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_39    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_40    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_41    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_42    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_43    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_44    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_45    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-07
         424791_46    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_47    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_48    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_49    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_50    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_51    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_52    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_53    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_54    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_55    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_56    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_57    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_58    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_59    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_60    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_61    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_62    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_63    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_64    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_65    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_66    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_67    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_68    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-06
         424791_69    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-16
         424791_70    128x24 Triangul   szhu44  R    2:56:19      1 hbnode-16
            424918    128x24 interact     jbos  R      41:36      1 hbnode-07
            424888    256x44     EDTA kamaryan  R    1:50:59      1 hbnode-25
            424936    256x44 interact jtsamson  R       3:15      1 hbnode-25
            403035 512x64-ib    pyscf   aluu10 PD       0:00      1 (Resources)
            403040 512x64-ib    pyscf   aluu10 PD       0:00      1 (ReqNodeNotAvail, UnavailableNodes:hbnode-40)
            403041 512x64-ib    pyscf   aluu10 PD       0:00      1 (ReqNodeNotAvail, UnavailableNodes:hbnode-40)
            403046 512x64-ib    pyscf   aluu10 PD       0:00      1 (ReqNodeNotAvail, UnavailableNodes:hbnode-40)
            400958 512x64-ib    pyscf  zliu373  R 10-21:56:46      1 hbnode-40
            401003 512x64-ib    pyscf  zliu373  R 10-21:16:00      1 hbnode-41
            424906 512x64-ib       qe kaljamal  R    1:24:22      2 hbnode-[38-39]
            422065 512x64-ib clown_dt  jwjohns  R 4-00:25:57      1 hbnode-31
            424714 512x64-ib 0d6fd122  jwjohns  R    6:01:55      1 hbnode-30
            424713 512x64-ib 0d6fd122  jwjohns  R    6:02:25      1 hbnode-30
            424723 512x64-ib 0d6fd122  jwjohns  R    5:36:55      1 hbnode-30
            424724 512x64-ib 0d6fd122  jwjohns  R    5:34:25      1 hbnode-30
            424737 512x64-ib 0d6fd122  jwjohns  R    4:47:42      1 hbnode-30
            424738 512x64-ib 0d6fd122  jwjohns  R    4:47:42      1 hbnode-30
            424748 512x64-ib 0d6fd122  jwjohns  R    4:05:52      1 hbnode-30
            424753 512x64-ib 0d6fd122  jwjohns  R    3:58:23      1 hbnode-30
            424754 512x64-ib 0d6fd122  jwjohns  R    3:58:23      1 hbnode-30
            424758 512x64-ib 0d6fd122  jwjohns  R    3:46:53      1 hbnode-30
            424762 512x64-ib 0d6fd122  jwjohns  R    3:35:53      1 hbnode-30
            424763 512x64-ib 0d6fd122  jwjohns  R    3:31:22      1 hbnode-30
            424764 512x64-ib 0d6fd122  jwjohns  R    3:30:52      1 hbnode-30
            424765 512x64-ib 0d6fd122  jwjohns  R    3:29:52      1 hbnode-30
            424768 512x64-ib 0d6fd122  jwjohns  R    3:18:53      1 hbnode-30
            424769 512x64-ib 0d6fd122  jwjohns  R    3:16:23      1 hbnode-30
            424779 512x64-ib 0d6fd122  jwjohns  R    3:13:53      1 hbnode-30
            424780 512x64-ib 0d6fd122  jwjohns  R    3:13:53      1 hbnode-30
            424784 512x64-ib 0d6fd122  jwjohns  R    3:07:53      1 hbnode-30
            424785 512x64-ib 0d6fd122  jwjohns  R    3:05:23      1 hbnode-30
            424861 512x64-ib 0d6fd122  jwjohns  R    2:52:23      1 hbnode-30
            424864 512x64-ib 0d6fd122  jwjohns  R    2:44:52      1 hbnode-30
            424865 512x64-ib 0d6fd122  jwjohns  R    2:40:23      1 hbnode-30
            424867 512x64-ib 0d6fd122  jwjohns  R    2:39:23      1 hbnode-30
            424874 512x64-ib 0d6fd122  jwjohns  R    2:18:22      1 hbnode-30
            424873 512x64-ib 0d6fd122  jwjohns  R    2:18:52      1 hbnode-30
            424875 512x64-ib 0d6fd122  jwjohns  R    2:14:52      1 hbnode-30
            424878 512x64-ib 0d6fd122  jwjohns  R    2:11:22      1 hbnode-30
            424883 512x64-ib 0d6fd122  jwjohns  R    2:00:52      1 hbnode-30
            424885 512x64-ib 0d6fd122  jwjohns  R    1:54:52      1 hbnode-29
            424886 512x64-ib 0d6fd122  jwjohns  R    1:51:52      1 hbnode-35
            424897 512x64-ib 0d6fd122  jwjohns  R    1:42:22      1 hbnode-30
            424900 512x64-ib 0d6fd122  jwjohns  R    1:33:23      1 hbnode-30
            424901 512x64-ib 0d6fd122  jwjohns  R    1:31:53      1 hbnode-30
            424902 512x64-ib 0d6fd122  jwjohns  R    1:29:53      1 hbnode-30
            424907 512x64-ib 0d6fd122  jwjohns  R    1:16:53      1 hbnode-40
            424910 512x64-ib 0d6fd122  jwjohns  R    1:13:23      1 hbnode-40
            424915 512x64-ib 0d6fd122  jwjohns  R      44:42      1 hbnode-40
            424916 512x64-ib 0d6fd122  jwjohns  R      44:42      1 hbnode-40
            424917 512x64-ib 0d6fd122  jwjohns  R      43:12      1 hbnode-41
            424919 512x64-ib 0d6fd122  jwjohns  R      40:36      1 hbnode-41
            424923 512x64-ib 0d6fd122  jwjohns  R      34:53      1 hbnode-41
            424926 512x64-ib 0d6fd122  jwjohns  R      26:52      1 hbnode-41
            424925 512x64-ib 0d6fd122  jwjohns  R      27:22      1 hbnode-41
            424928 512x64-ib 0d6fd122  jwjohns  R      15:53      1 hbnode-41
            424930 512x64-ib 0d6fd122  jwjohns  R      15:23      1 hbnode-41
            424931 512x64-ib 0d6fd122  jwjohns  R       8:53      1 hbnode-41
            424932 512x64-ib 0d6fd122  jwjohns  R       7:53      1 hbnode-41
            424933 512x64-ib 0d6fd122  jwjohns  R       7:53      1 hbnode-41
            424935 512x64-ib 0d6fd122  jwjohns  R       4:23      1 hbnode-41
397752_[2699-3578% lab-colib ukraine_ ogarci12 PD       0:00      1 (Nodes required for job are DOWN, DRAINED or reserved for jobs in higher priority partitions)
       397752_2698 lab-colib ukraine_ ogarci12  R       6:53      1 hbnode-40
       397752_2697 lab-colib ukraine_ ogarci12  R      15:53      1 hbnode-40
       397752_2696 lab-colib ukraine_ ogarci12  R      16:23      1 hbnode-40
       397752_2695 lab-colib ukraine_ ogarci12  R      27:52      1 hbnode-40
       397752_2692 lab-colib ukraine_ ogarci12  R      38:53      1 hbnode-40
       397752_2693 lab-colib ukraine_ ogarci12  R      38:53      1 hbnode-40
       397752_2694 lab-colib ukraine_ ogarci12  R      38:53      1 hbnode-40
```

You can check the status of only your jobs (queued and running) with ```squeue -u <username>```

```
squeue -u mglasena

```

```
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            407851    128x24 genotype mglasena  R 5-21:24:31      1 hbnode-17
```

My job (id: 407851, name: genotype) has been running for 5 days, 21 hours, 24 minutes, and 31 seconds on hbnode-17. Let's get more detail about this job using ```scontrol show job <job_id>```

```
scontrol show job 407851
```

```
JobId=407851 JobName=genotype_d214
   UserId=mglasena(236371) GroupId=ucsc_p_all_usr(100000) MCS_label=N/A
   Priority=10018522 Nice=0 Account=128x24 QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=5-21:51:00 TimeLimit=7-00:00:00 TimeMin=N/A
   SubmitTime=2025-01-21T15:46:41 EligibleTime=2025-01-21T15:46:41
   AccrueTime=2025-01-21T15:46:41
   StartTime=2025-01-21T16:47:16 EndTime=2025-01-28T16:47:16 Deadline=N/A
   PreemptEligibleTime=2025-01-21T16:47:16 PreemptTime=None
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2025-01-21T16:47:16 Scheduler=Backfill
   Partition=128x24 AllocNode:Sid=hb-login:4134358
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=hbnode-17
   BatchHost=hbnode-17
   NumNodes=1 NumCPUs=24 NumTasks=1 CPUs/Task=24 ReqB:S:C:T=0:0:*:*
   ReqTRES=cpu=24,mem=120G,node=1,billing=24
   AllocTRES=cpu=24,mem=120G,node=1,billing=24
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=24 MinMemoryNode=0 MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=NO Contiguous=0 Licenses=(null) Network=(null)
   Command=/hb/scratch/mglasena/urchin_seq_2024/d214_haplotypecaller.sh
   WorkDir=/hb/scratch/mglasena/urchin_seq_2024
   StdErr=/hb/scratch/mglasena/urchin_seq_2024/genotype_d214.err
   StdIn=/dev/null
   StdOut=/hb/scratch/mglasena/urchin_seq_2024/genotype_d214.out
   Power=
   TresPerTask=cpu:24
   MailUser=mglasena@ucsc.edu MailType=INVALID_DEPEND,BEGIN,END,FAIL,REQUEUE,STAGE_OUT
   ```

You can see that I have set a time limit of 7-00:00:00 for this job. I requested 24 CPUs and 120G of RAM. The slurm script I submitted was /hb/scratch/mglasena/urchin_seq_2024/d214_haplotypecaller.sh. 

## 2. Submit a Slurm job

To submit a job to run on the cluster, create a bash script called <job_name>.sh. Submit the job using the ```sbatch <job_name>.sh``` command. In your bash script, you need to include a header with arguments for Slurm. 

```
#!/bin/bash
#SBATCH --job-name=genotype
#SBATCH --mail-type=ALL
#SBATCH --mail-user=mglasena@ucsc.edu
#SBATCH --output=genotype_%A_%a.out
#SBATCH --error=genotype_%A_%a.err
#SBATCH --mem=32G
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH --time=3-0
#SBATCH --array=0-32%9
#SBATCH --partition=lab-colibri
#SBATCH --qos=pi-jkoc
#SBATCH --account=pi-jkoc
```

The array job above specifies 8 CPUs per array task. On the 128x24 Hummingbird partitions, there is a 72 CPU core maximum per user, so only 9 array tasks will run at a time. Because the lab-colibri partition does not have a hard-coded CPU limit, it will run as many array tasks as there's space for. You can specify to only run 9 array tasks at a time using ```#SBATCH --array=0-32%9```.


## 3. Interactive Slurm Jobs

You can run an interactive job using the ```srun``` commmand. This is great for debugging 

```
srun --pty --partition=lab-colibri --mem=20G --ntasks=1 --cpus-per-task=8 --time=1-00:00:00 --qos=pi-jkoc --account=pi-jkoc --job-name=debug /bin/bash/
```

## 4. Efficiency

You can see how efficient your slurm jobs were using the following command: ```seff <job_id>```

Let's inspect one of my recent jobs. This is a different job from the one above. Note that the ```seff``` command typically doesn't work while a job is still running. 

```
seff 398476_150
```

```
Job ID: 402098
Array Job ID: 398476_150
Cluster: hbhpc
User/Group: mglasena/ucsc_p_all_usr
State: COMPLETED (exit code 0)
Nodes: 1
Cores per node: 8
CPU Utilized: 1-06:23:41
CPU Efficiency: 80.49% of 1-13:45:44 core-walltime
Job Wall-clock time: 04:43:13
Memory Utilized: 26.46 GB
Memory Efficiency: 66.15% of 40.00 GB
```

Memeory Utilized: Array task 150 of slurm job 398476 only used 26.46 GB of RAM. Next time I run a similar job, I can reduce requested RAM from 40.00 GB to 30.00 GB. 

CPU Efficiency is the amount of time the requested CPUs were actively doing work relative to the amount of time they were idle. 
In this case, 80% of the reserved CPU time was actively used for computations, and the remaining ~20% was idle. This is fairly efficient, but next time I could consider requesting slightly fewer CPUs. Note that CPU usage can vary drastically during the job if you have multiple commands in your slurm job. Efficiency will be low when only one or a few steps require many CPU, and the other steps cannot make use of multiple CPU. 


