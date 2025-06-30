# Hummingbird Tutorial
Matt's tutorial for UCSC's Hummingbird HPC cluster.

## Table of Contents

- [1. Login](#1-login)
- [2. Basic slurm commands](#2-basic-slurm-commands)
- [3. Submit a Slurm job](#3-submit-a-slurm-job-dont-test-these-commands-now)
  - [3.1 Create a slurm script](#31-create-a-slurm-script)
  - [3.2 Submit the slurm script](#32-submit-the-slurm-script)
  - [3.3 Cancel a slurm job](#33-cancel-a-slurm-job)
- [4. Interactive Slurm Jobs](#4-interactive-slurm-jobs)
- [5. Efficiency](#5-efficiency)

A cluster, or supercomputer, is a group of computers that work together and function as a single system. The advantage of using Hummingbird is that it has much more memory and storage than your own personal computers. We can submit jobs to run on hummingbird and will be notified by email when they complete. We do not have to keep our computer running or monitor the progress. 

We interact with Hummingbird via the command line interface using UNIX commands. UNIX is an operating system that includes a collection of built-in tools and commands. Here is a great commandline tutorial if you have no prior experience: https://www.codecademy.com/learn/learn-the-command-line.

The commands used to submit and monitor jobs on Hummingbird are mostly not Hummingbird-specific. Hummingbird uses the Slurm workload manager software. Slurm (Simple Linux Utility for Resource Management) is a job scheduler that automates the process of allocating resources (i.e., hardware) for users' computational tasks. There is tons of content on slurm on the internet.

Learn more about slurm at:
1. Hummingbird homepage, https://hummingbird.ucsc.edu/, and Hummingbird Getting Started Page, https://hummingbird.ucsc.edu/getting-started/
3. Stanford slurm tutorial: https://login.scg.stanford.edu/tutorials/job_scripts/
4. ChatGPT

If you are a regular user, you should join the Hummingbird Slack Community: https://ucschummingbi-lph3072.slack.com/join/shared_invite/zt-19mbwqvx1-GqguQcumVBLss~nzjOHAYg#/shared-invite/email

The Humminbird team also has weekly office hours on Thursday from 1:00 PM - 2:00 PM via Zoom: https://hummingbird.ucsc.edu/documentation/hummingbird-open-office-hour/

You can monitor cluster usage at https://hummingbird.ucsc.edu/current-usage/

There are example slurm scripts on Hummingbird located at ```/hb/software/scripts```

## 1. Login
If not on the campus WiFi, you will need to be connected to the campus VPN. 

To log in to hummingbird, first open the terminal application (Mac users) or PuTTY (Windows users). We will use the ssh command to access Hummingbird, which stands for secure shell and provides a secure connection between your computer and the Hummingbird server. To login, you will use the following command:

```
ssh <your_cruzid>@hb.ucsc.edu
```

Replace <your_cruzid> with your UCSC username in your command. You will be prompted to enter your password. For security measures, you will not be able to see the characters you are entering. Type your password and press enter.

The following message should appear on your screen.

```
 _                               _             _     _         _
| |                             (_)           | |   (_)       | |
| |__  _   _ _ __ ___  _ __ ___  _ _ __   __ _| |__  _ _ __ __| |
| '_ \| | | | '_ ` _ \| '_ ` _ \| | '_ \ / _` | '_ \| | '__/ _` |
| | | | |_| | | | | | | | | | | | | | | | (_| | |_) | | | | (_| |
|_| |_|\__,_|_| |_| |_|_| |_| |_|_|_| |_|\__, |_.__/|_|_|  \__,_|
                                          __/ |
                                        |    /
                                        |___/
-----------------------------------------------------------------------------
!! POLICIES - READ !!
** Faiure to adhere to these policies will result in job cancellation. **
-----------------------------------------------------------------------------
1) JOBS & QUEUES
** YOU MUST USE THE SLURM BATCH SYSTEM TO RUN SOFTWARE **
- 72 CPU core maximum per user
- Always apply time limits to slurm script (#SBATCH --time=)
- For non-exclusive access to a node apply BOTH memory & cpu limits

2) DATA TRANSFERS
- All data transfers can be done directly on hb.ucsc.edu or via Globus

3) HOME QUOTA, SCRATCH & DATA POLICY
- Disk quota per user (home):  1TB
- /hb/scratch is not backed up and data older than 120 days may be deleted
- Data and Backup Policy: https://hummingbird.sites.ucsc.edu/documentation/hummingbird-data-storage-and-backup-policy/

4) GETTING HELP
** Have questions? Need help? Want to speak to an expert? **
- Join the Hummingbird Zoom-in Help Clinic Thursdays at 1PM PDT
- https://ucsc.zoom.us/j/93463299124?pwd=RFZvZzlYSjIzNnoxblFsRUV6aTZGZz09 (UCSC login required)
- Put in a ticket by emailing hummingbird@ucsc.edu
-----------------------------------------------------------------------------
** More information at https://hummingbird.ucsc.edu/ **
-----------------------------------------------------------------------------
** Join us on the Hummingbird-cluster Slack server!! **

Join the community to get real-time support from your admins and collaborators.

https://join.slack.com/t/ucschummingbi-lph3072/shared_invite/zt-19mbwqvx1-GqguQcumVBLss~nzjOHAYg
-----------------------------------------------------------------------------
Last login: Mon Jun 30 13:34:30 2025 from 128.114.226.87
```

After connecting to Hummingbird, you are on the login node. This is a place to navigate and edit files and monitor jobs. Do not run code/scripts on the login node. It will slow it down for all users. 
![compute_cluster](https://github.com/user-attachments/assets/cdbfeb86-011c-4b89-85cf-e19cf8bf67e4)

When you log in to Hummingbird, you are automatically taken to your home directory. To see the path of your working directory, use the pwd command, which stands for "print working directory."

```
pwd
```

After running this command, /hb/home/<your_cruzid> will be printed to the terminal. This is your personal/home directory, where you can store files and data. Hummingbird users have a 1 TB storage quote in their home directory. I reccomend using /hb/home/ for longer-term storage, including stable files and software installs. 

You'll want to work mostly in your scratch directory, at /hb/scratch/<cruz_id>, where the is no storage quota. This is a great location to test new scripts and to store intermediate files.

```
cd /hb/scratch/<your_cruzid>
```

Let's learn more about the cluster. 

``` sinfo ```

```
PARTITION        AVAIL  TIMELIMIT  NODES  STATE NODELIST
build               up   infinite      1   idle hbnode-05
instruction         up   12:00:00      3   idle hbnode-[03-05]
128x24*             up   infinite     15    mix hbnode-[06,09-18,20-23]
128x24*             up   infinite      3  alloc hbnode-[07-08,19]
256x44              up   infinite      1    mix hbnode-25
96x24gpu4           up   infinite      1    mix hbnode-24
512x64-ib           up   infinite      1    mix hbnode-38
512x64-ib           up   infinite      8  alloc hbnode-[29-36]
512x64-ib           up   infinite      4   idle hbnode-[37,39-41]
lab-colibri         up   infinite      1    mix hbnode-38
lab-colibri         up   infinite      1   idle hbnode-39
lab-colibri-hmem    up   infinite      1   idle hbnode-40
```

The different partitions are listed. Partitions are nodes with different configurations. 

## 2. Basic slurm commands 
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

Check the status of jobs by users (queued and running) with ```squeue -u <username>```

```
squeue -u mglasena

```

```
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            407851    128x24 genotype mglasena  R 5-21:24:31      1 hbnode-17
```

My job (id: 407851, name: genotype) has been running for 5 days, 21 hours, 24 minutes, and 31 seconds on hbnode-17, which is part of the 128x24 partition. Let's get more detail about this job using ```scontrol show job```.

```scontrol show job <job_id>```

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

## 3. Submit a Slurm job (don't test these commands now)

### 3.1 Create a slurm script. 

Let's use the nano text editor to 

```nano <script_name>.sh```

```
#!/bin/bash
#SBATCH --job-name=test
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<cruzid>@ucsc.edu
#SBATCH --output=test_%J.out
#SBATCH --error=test_%J.err
#SBATCH --partition=128x24
#SBATCH --nodes=1
#SBATCH --mem=40G   
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --time=1-0
```

### 3.2 Submit the slurm script

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

### 3.3 Cancel a slurm job

You can cancel a running job at any moment using ```scancel <job_id>```

If you accidentally run a job on the login node by mistake, you can kill the process with Ctl + c or exiting the shell window (automatically closes connection). 

## 4. Interactive Slurm Jobs

You can run an interactive job using the ```srun``` commmand. This is great for debugging 

```
srun --pty --partition=lab-colibri --mem=20G --ntasks=1 --cpus-per-task=8 --time=1-00:00:00 --qos=pi-jkoc --account=pi-jkoc --job-name=debug /bin/bash
```

## 5. Efficiency

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


