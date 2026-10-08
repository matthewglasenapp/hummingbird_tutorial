<br/>

<h1 align="center">Hummingbird Tutorial</h1>

<p align="center">
  <b>Matt's guide to UCSC's Hummingbird and Elkhorn clusters</b>
</p>

<br/>

<p align="center">
  <a href="#part-i-getting-started"><picture><source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/Getting_Started-1F2937?style=for-the-badge&logo=gnubash&logoColor=white"><img src="https://img.shields.io/badge/Getting_Started-E5E7EB?style=for-the-badge&logo=gnubash&logoColor=1F2937" alt="Getting Started"></picture></a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="#part-ii-running-jobs"><picture><source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/Running_Jobs-1F2937?style=for-the-badge&logo=slurm&logoColor=white"><img src="https://img.shields.io/badge/Running_Jobs-E5E7EB?style=for-the-badge&logo=slurm&logoColor=1F2937" alt="Running Jobs"></picture></a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="#part-iii-software"><picture><source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/Software-1F2937?style=for-the-badge&logo=anaconda&logoColor=white"><img src="https://img.shields.io/badge/Software-E5E7EB?style=for-the-badge&logo=anaconda&logoColor=1F2937" alt="Software"></picture></a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="#part-iv-scaling-up"><picture><source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/Scaling_Up-1F2937?style=for-the-badge&logo=python&logoColor=white"><img src="https://img.shields.io/badge/Scaling_Up-E5E7EB?style=for-the-badge&logo=python&logoColor=1F2937" alt="Scaling Up"></picture></a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="#cheat-sheet"><picture><source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/Cheat_Sheet-1F2937?style=for-the-badge&logo=readthedocs&logoColor=white"><img src="https://img.shields.io/badge/Cheat_Sheet-E5E7EB?style=for-the-badge&logo=readthedocs&logoColor=1F2937" alt="Cheat Sheet"></picture></a>
</p>

<br/>

> [!WARNING]
> This is an unofficial guide, written from my own experience using Hummingbird and Elkhorn. It is not maintained by the UCSC HPC team, and parts of it may be out of date. For official help, see [Getting Help](#1-getting-help).

<details>
<summary><b>Table of Contents</b></summary>

**[Part I. Getting Started](#part-i-getting-started)**
1. [Getting Help](#1-getting-help)
2. [What Is a Cluster?](#2-what-is-a-cluster)
3. [Hummingbird and Elkhorn](#3-hummingbird-and-elkhorn)
4. [Logging In](#4-logging-in)
5. [Where to Put Your Files](#5-where-to-put-your-files)
6. [Moving Files](#6-moving-files)

**[Part II. Running Jobs](#part-ii-running-jobs)**

7. [Partitions](#7-partitions)
8. [Watching the Queue](#8-watching-the-queue)
9. [Writing and Submitting a Job](#9-writing-and-submitting-a-job)
10. [Interactive Jobs](#10-interactive-jobs)
11. [Checking Job Efficiency](#11-checking-job-efficiency)

**[Part III. Software](#part-iii-software)**

12. [Modules](#12-modules)
13. [Conda](#13-conda)
14. [Permissions](#14-permissions)

**[Part IV. Scaling Up](#part-iv-scaling-up)**

15. [Array Jobs](#15-array-jobs)
16. [Elkhorn Daily Tips](#16-elkhorn-daily-tips)
17. [Troubleshooting](#17-troubleshooting)

**[Cheat Sheet](#cheat-sheet)**

</details>

<br/>

# Part I. Getting Started

## 1. Getting Help

**Asking the HPC team**

- Email **[help@ucsc.edu](mailto:help@ucsc.edu)** for help with Hummingbird or Elkhorn. See the [contact page](https://hummingbird.ucsc.edu/contact-us/).
- Be as specific as you can. Include links to any software you need installed, the log files of the job that failed, and the terminal output that shows the error.
- Do not email individual staff members, and do not use hummingbird@ucsc.edu. That address is retired.
- If you use the clusters often, join the [Hummingbird and Elkhorn Slack](https://ucschummingbi-lph3072.slack.com/join/shared_invite/zt-19mbwqvx1-GqguQcumVBLss~nzjOHAYg#/shared-invite/email).
- The Hummingbird team holds [office hours](https://hummingbird.ucsc.edu/documentation/hummingbird-open-office-hour/) on Zoom, Thursdays from 1:00 PM to 2:00 PM.

**Learning Slurm**

- The [Hummingbird homepage](https://hummingbird.ucsc.edu/) and its [Getting Started page](https://hummingbird.ucsc.edu/getting-started/)
- The [Stanford Slurm tutorial](https://login.scg.stanford.edu/tutorials/job_scripts/)
- Claude and ChatGPT are great for troubleshooting common Slurm issues.

**Useful resources**

- [Current cluster usage](https://hummingbird.ucsc.edu/current-usage/)
- Example Slurm scripts on Hummingbird, in `/hb/software/scripts`

## 2. What Is a Cluster?

A cluster, or supercomputer, is a group of computers that work together and function as a single system. The advantage of using Hummingbird is that it has much more memory and storage than your own personal computers. We can submit jobs to run on Hummingbird and get be notified by email when they complete. We do not have to keep our computer running or monitor the progress.

We interact with Hummingbird via the command line interface using UNIX commands. UNIX is an operating system that includes a collection of built-in tools and commands. Here is a great command line tutorial if you have no prior experience: https://www.codecademy.com/learn/learn-the-command-line.

The commands used to submit and monitor jobs on Hummingbird are mostly not Hummingbird-specific. Hummingbird uses the Slurm workload manager software. Slurm (Simple Linux Utility for Resource Management) is a job scheduler that automates the process of allocating resources (i.e., hardware) for users' computational tasks. There is tons of content on Slurm on the internet (see [Getting Help](#1-getting-help)).

Every cluster has two kinds of computers. You use the **login node** to navigate the filesystem and submit jobs, which run on **compute nodes**. Slurm decides which compute node runs your job based on a set of rules created by the Hummingbird admin.

<p align="center">
  <img src="https://github.com/user-attachments/assets/cdbfeb86-011c-4b89-85cf-e19cf8bf67e4" alt="compute_cluster" width="700">
</p>



> [!WARNING]
> Do not run code or scripts on the login node. It slows it down for all users. Use `sbatch` or `salloc`, which are covered in [Part II](#part-ii-running-jobs).

## 3. Hummingbird and Elkhorn

Hummingbird and Elkhorn are two separate Slurm clusters. Each has its own login node, its own compute nodes, and its own partitions, and a job runs only on the nodes of the cluster you submit it from. The two clusters share one file system, so your home and scratch directories are the same on both. The Slurm commands are the same on both.

| | Hummingbird | Elkhorn |
|---|---|---|
| Compute nodes | Shared campus nodes, open to all Hummingbird users | Nodes bought by individual labs. Each lab has priority on its own nodes, and other users can run on idle nodes through windfall |
| Log in with | `ssh <your_cruzid>@hb.ucsc.edu` | `ssh <your_cruzid>@elkhorn.ucsc.edu` |
| Cornejo-Kelley nodes | | node-38 and node-39 (lab-colibri), node-40 (lab-colibri-hmem) |
| Shared queue | 128x24 and other partitions | windfall, which spans every node on Elkhorn |
| Home | `/home/<your_cruzid>` | The same directory |
| Scratch | `/scratch/<your_cruzid>` | The same directory |

Both systems share your home and scratch directories, so a file you write on one is there on the other.

Undergrad interns should start with Hummingbird and only use Elkhorn with their mentor's permission. Grads should familiarize themselves with Elkhorn in order to work with our lab's private nodes. The windfall partition of Elkhorn is great if you are stuck in the Hummingbird or Elkhorn lab-colibri queue. Elkhorn usually has a lot of free CPU, but this may change as the system onboards more users. 

Access to Elkhorn requires your PI to have sponsored you. You can check whether you are linked to the pi-jkoc lab account with the following command:

```
sacctmgr show assoc user=$USER format=cluster,account,partition,qos%30,share
```

## 4. Logging In

If you are not on the campus WiFi, you will need to be connected to the campus VPN. The campus VPN now requires your device to have the campus security bundle (Verified Access) installed. https://its.ucsc.edu/get-support/it-guides/verified-access/

To log in, first open the terminal application (Mac users) or PuTTY (Windows users). Ue the `ssh` command, which stands for secure shell. `ssh` provides a secure connection between your computer and the server. To log in to Hummingbird, you will use the following command:

```
ssh <your_cruzid>@hb.ucsc.edu
```

Replace `<your_cruzid>` with your UCSC username in your command. You will be prompted to enter your password. For security measures, you will not be able to see the characters you are entering. 


Repeat: Characters will not print to the screen as you type your password!

Type your password and press enter.

To log in to Elkhorn, use `elkhorn.ucsc.edu` rather than `hb.ucsc.edu`.

```
ssh <your_cruzid>@elkhorn.ucsc.edu
```

The prompt shows which system you are on, for example `[mglasena@hb ~]$` on Hummingbird and `[mglasena@elkhorn ~]$` on Elkhorn.

A welcome message appears on your screen.

<details>
<summary><b>Example Hummingbird login message</b></summary>

```
 _                               _             _     _         _ 
| |__  _   _ _ __ ___  _ __ ___ (_)_ __   __ _| |__ (_)_ __ __| |
| '_ \| | | | '_ ` _ \| '_ ` _ \| | '_ \ / _` | '_ \| | '__/ _` |
| | | | |_| | | | | | | | | | | | | | | | (_| | |_) | | | | (_| |
|_| |_|\__,_|_| |_| |_|_| |_| |_|_|_| |_|\__, |_.__/|_|_|  \__,_|
                                         |___/                   
-----------------------------------------------------------------------------
** Join us on the UCSC HPC Slack server!! **

Join the community to get real-time support from your admins and collaborators.

https://join.slack.com/t/ucschummingbi-lph3072/shared_invite/zt-19mbwqvx1-GqguQcumVBLss~nzjOHAYg
-----------------------------------------------------------------------------
Last login: Fri Oct  2 16:06:57 2026 from 128.114.224.17
[mglasena@hb ~]$
```

</details>

After connecting, you are on the login node. This is a place to navigate and edit files and monitor jobs. Do not run code or scripts on the login node. It will slow it down for all users.

## 5. Where to Put Your Files

When you log in, you are automatically taken to your home directory. To see the path of your working directory, use the `pwd` command, which stands for "print working directory."

```
pwd
```

After running this command, `/home/<your_cruzid>` will be printed to the terminal. This is your personal home directory, where you can store files and data. Hummingbird users have a 1 TB storage quota in their home directory. 

I recommend using your home directory for longer-term storage, including stable files and software installs. For preliminary analysis and testing, you'll want to work mostly in your scratch directory, at `/scratch/<your_cruzid>`, where there is no storage quota. This is a great location to test new scripts and to store intermediate files. 

> [!IMPORTANT]
> Check in with your mentor about where you should be storing important files, such as raw data, stable scripts, and final output files.

```
cd /scratch/<your_cruzid>
```

| Location | Use it for | Notes |
|---|---|---|
| `/home/<your_cruzid>` | Software installs, scripts, files you keep | 1 TB quota |
| `/scratch/<your_cruzid>` | Running jobs and intermediate files | Not backed up. Old files may be deleted, see the [data and backup policy](https://hummingbird.sites.ucsc.edu/documentation/hummingbird-data-storage-and-backup-policy/) |

> [!WARNING]
> Always write `/scratch/<your_cruzid>` in your scripts. `/hb/scratch` also works on the login nodes, but it does not exist on the compute nodes, so a job that uses it fails.

> [!CAUTION]
> Scratch is not backed up. Keep a copy of anything you cannot regenerate in your home directory, in one of the lab folders, or on your own computer.

## 6. Moving Files

For transferring files to and from Hummingbird, you can use `scp` or `sftp`. Note that if you are not on the campus WiFi, you must be connected to the VPN. Run `scp` on your own computer, not on the cluster.

Example of a transfer from Matt's Desktop to Hummingbird:

```
scp /Users/matt/Desktop/test.txt mglasena@hb.ucsc.edu:/scratch/mglasena/
```

Example of a transfer from Hummingbird to Matt's Desktop:

```
scp mglasena@hb.ucsc.edu:/scratch/mglasena/test.txt /Users/matt/Desktop/
```

There are desktop applications with nice graphical interfaces for file transfer and management (e.g., Cyberduck: https://cyberduck.io/).

For larger data transfers, see Hummingbird's Globus recommendations: https://hummingbird.ucsc.edu/documentation/

<br/>

# Part II. Running Jobs

## 7. Partitions

You can see the available partitions with `sinfo`. Partitions are groups of nodes with different configurations. The `STATE` column shows whether a node is free (`idle`), partly used (`mix`), full (`alloc`), or being taken out of service (`drng`).

### 7.1 Hummingbird Partitions

```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
128x24*      up   infinite     11    mix node-[06,08-10,12,15,18-19,21-23]
128x24*      up   infinite      4  alloc node-[07,11,13-14]
128x24*      up   infinite      3   idle node-[16-17,20]
96x24gpu4    up   infinite      1    mix node-24
256x44       up   infinite      1   mix- node-25
```

`128x24` is the default partition (the `*`). `96x24gpu4` has GPUs, and `256x44` has more memory.

> [!IMPORTANT]
> On the 128x24 Hummingbird partitions, there are hard-coded CPU limits (no more than 72 CPUs per user at a time). If you submit an array job on the 128x24 partition, the job scheduler will regulate the number of tasks allowed to run simultaneously.

To use the 128x24 partition, include the following in your Slurm script header.

```
#SBATCH --partition=128x24
```

### 7.2 Elkhorn Partitions

```
PARTITION        AVAIL  TIMELIMIT  NODES  STATE NODELIST
windfall*           up   infinite      1   drng node-114
windfall*           up   infinite     15    mix node-[37-41,44,46-48,50-52,104-105,117]
windfall*           up   infinite     64  alloc node-[29-30,33-36,42-43,45,49,53-97,107-113,115-116]
windfall*           up   infinite      9   idle node-[31-32,98-103,106]
512x64              up   infinite      2  alloc node-[29-30]
lab-colibri         up   infinite      2    mix node-[38-39]
lab-colibri-hmem    up   infinite      1    mix node-40
```

| Partition | Nodes | Who can use it | Header lines |
|---|---|---|---|
| `lab-colibri` | node-38, node-39 | The Cornejo and Kelley labs, with priority. Our jobs here are never preempted | `--partition=lab-colibri --account=pi-jkoc --qos=pi-jkoc` |
| `lab-colibri-hmem` | node-40 | The Cornejo and Kelley labs, with priority. Our jobs here are never preempted | `--partition=lab-colibri-hmem --account=pi-jkoc --qos=pi-jkoc` |
| `windfall` | Every Elkhorn node, including ours | Every Elkhorn user. A job on another lab's node is preempted when that lab needs the node | `--partition=windfall` |
| `512x64` | node-29, node-30 | Another lab, with priority. Everyone else reaches these nodes through windfall | |

Any Elkhorn user can run on node-38, node-39 and node-40 through windfall when we are not using them. When we submit to `lab-colibri` or `lab-colibri-hmem`, those windfall jobs are preempted to make room for ours.

#### Colibri

Our lab has three private nodes, two on the partition `lab-colibri` (node-38, node-39) and one high memory node on the partition `lab-colibri-hmem` (node-40). node-38 and node-39 have 112 CPUs and 1000 GB RAM each. node-40 is a high memory node with 112 CPUs and 2000 GB RAM.

> [!IMPORTANT]
> There are no hard-coded CPU limits on the lab-colibri and lab-colibri-hmem partitions. Please be considerate of how much compute resources you are allocating at a single time. If the nodes are idle and you have a high priority job, feel free to allocate a lot of resources. If there are many people in the queue and your job is not time-sensitive, try to occupy fewer CPUs to allow other lab members to access the partitions. 

Because the lab-colibri partition does not have a hard-coded CPU limit, it will run as many array tasks as physically possible given the available CPU and RAM. You can limit the number of array tasks that run at the same with the following slurm header line:

```
#SBATCH --array=0-999%10
```

This line creates 1,000 array tasks (0-based indexing), but allows no more than 10 tasks to run simultaneously. [Section 15](#15-array-jobs) covers array jobs.

To target node-38 and/or node-39, include the following in your Slurm script header.

```
#SBATCH --partition=lab-colibri
#SBATCH --qos=pi-jkoc
#SBATCH --account=pi-jkoc
```

node-40 is on a different partition, `lab-colibri-hmem`. To target node-40, include the following in your Slurm script header.

```
#SBATCH --partition=lab-colibri-hmem
#SBATCH --qos=pi-jkoc
#SBATCH --account=pi-jkoc
```

#### windfall

In addition to our lab's nodes, we also have access to other lab groups' nodes when they're idle! The windfall partition (`--partition=windfall`) spans every node in the Elkhorn cluster, and it has no CPU limit. It is the default partition on Elkhorn (the `*` in `sinfo`).

> [!WARNING]
> If your job is running on another lab's node and someone from that lab requests it, your job is canceled and does not restart. So you need to build your own verification that your jobs actually finished, or set up requeuing ([section 16.1](#161-requeuing)).

This works the same with our nodes. When ours are idle, people from outside the group can end up on our nodes via windfall. If we then submit a job that needs those resources, theirs gets canceled, and ours runs instead (usually within a minute). Importantly, we can't get pre-empted or canceled on our own nodes.

```
#SBATCH --partition=windfall
#SBATCH --requeue
```

## 8. Watching the Queue

Check the queue to see what jobs are currently running.

```
squeue
```

<details>
<summary><b>Example squeue output from Elkhorn</b></summary>

```
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5651771  windfall axi3d-re   jowolf PD       0:00     16 (Resources)
           5651638  windfall meas_icT   jowolf PD       0:00      1 (Priority)
           5651673  windfall s3a300nu      xiz PD       0:00      2 (Dependency)
           5651661  windfall  s3a300e      xiz PD       0:00      2 (Dependency)
           5651578  windfall fxs_ext_      xiz PD       0:00      1 (Dependency)
           5651772  windfall post_icT   jowolf PD       0:00      1 (Dependency)
           5651769  windfall post_icT   jowolf PD       0:00      1 (Dependency)
           5651773  windfall meas_icT   jowolf PD       0:00      1 (Dependency)
           5651770  windfall meas_icT   jowolf PD       0:00      1 (Dependency)
           5645485  windfall W0047_tw cphilli4  R 8-23:19:17      2 node-[79-80]
           5639285  windfall orca_tes jjuanita  R 15-04:48:26      1 node-45
           5597657  windfall orca_tes jjuanita  R 21-02:16:43      1 node-49
           5647920  windfall pyscf_te jjuanita  R 5-22:48:02      1 node-48
           5647921  windfall pyscf_te jjuanita  R 5-22:47:02      1 node-50
           5647922  windfall pyscf_te jjuanita  R 5-22:40:15      1 node-51
           5647924  windfall pyscf_te jjuanita  R 5-22:35:34      1 node-52
           5647926  windfall pyscf_te jjuanita  R 5-16:43:23      1 node-46
           5647952  windfall W0047_pa cphilli4  R 5-05:15:06      2 node-[77-78]
           5647979  windfall pyscf_te jjuanita  R 4-17:54:04      1 node-47
           5648724  windfall W0047_En cphilli4  R 2-01:23:59      2 node-[115-116]
           5648727  windfall W0047_Si cphilli4  R 2-01:22:53      2 node-[42-43]
           5648728  windfall 2M2244_Q cphilli4  R 2-01:22:14      2 node-[35-36]
           5648734  windfall 2M2244_S cphilli4  R 2-01:20:45      2 node-[55-56]
           5648732  windfall 2M2244_F cphilli4  R 2-01:21:13      2 node-[53-54]
           5648736  windfall W0047_pa cphilli4  R 2-01:19:45      2 node-[57-58]
           5648740  windfall W0047_pa cphilli4  R 2-01:18:44      2 node-[61-62]
           5648739  windfall W0047_pa cphilli4  R 2-01:19:10      2 node-[59-60]
           5648741  windfall 2M2244_p cphilli4  R 2-01:17:45      2 node-[63-64]
           5648743  windfall 2M2244_p cphilli4  R 2-01:16:43      2 node-[67-68]
           5648742  windfall 2M2244_p cphilli4  R 2-01:17:14      2 node-[65-66]
           5648744  windfall 2M2244__ cphilli4  R 2-01:16:06      2 node-[69-70]
           5651012  windfall ethan_sc eschreye  R   14:30:30     16 node-[81-92,107-110]
           5651459  windfall 331_QZ_S   aluu10  R    7:32:52      1 node-41
           5651543  windfall W0047_Fo cphilli4  R    5:51:44      2 node-[75-76]
           5651544  windfall W0047_Qu cphilli4  R    5:51:14      2 node-[29-30]
           5651575  windfall fxs_ext_      xiz  R    4:57:43      1 node-104
           5651768  windfall axi3d-re   jowolf  R      59:46     12 node-[93-103,106]
           5651660  windfall  s3a300e      xiz  R    2:35:06      2 node-[31-32]
           5651664  windfall   s3a30e      xiz  R    2:33:44      2 node-[112-113]
           5651665  windfall   s3a10e      xiz  R    2:33:44      2 node-[71-72]
           5651672  windfall s3a300nu      xiz  R    2:15:44      2 node-[73-74]
           5651765  windfall orca_tes jjuanita  R      21:16      1 node-44
           5651777  windfall 2M2244_E cphilli4  R      58:46      2 node-[37,111]
           5651776  windfall 331_QZ_S   aluu10  R    1:03:46      1 node-40
           5651612  windfall interact mescob11  R    3:39:30      1 node-105
```

</details>

Check the status of a specific user's jobs (queued and running) with `squeue -u <username>`.

```
squeue -u mglasena
```

```
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            407851    128x24 genotype mglasena  R 5-21:24:31      1 hbnode-17
```

My job (id 407851, name genotype) has been running for 5 days, 21 hours, 24 minutes, and 31 seconds on hbnode-17, which is part of the 128x24 partition. You can get more info about any job, including other users' jobs, with `scontrol`

```
scontrol show job 407851
```

<details>
<summary><b>Example scontrol output from Hummingbird (2025)</b></summary>

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

</details>

You can see that I have set a time limit of 7-00:00:00 for this job. I requested 24 CPUs and 120G of RAM. The Slurm script I submitted was `/hb/scratch/mglasena/urchin_seq_2024/d214_haplotypecaller.sh`.

## 9. Writing and Submitting a Job

### 9.1 Create a Slurm Script

Either create slurm scripts on your local machine and transfer them to hummingbird, or write them on Hummingbird using a text editor, such as `nano`, `vim`, or `emacs`. `nano` is the most user-friendly option for beginners. 

```
nano test.sh
```

Here is an example slurm header:

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

To run the same job on Elkhorn, change the partition lines. See [section 7.2](#72-elkhorn-partitions).

What each header line means:

| Line | Meaning |
|---|---|
| `--job-name` | The name `squeue` shows |
| `--mail-type=ALL`, `--mail-user` | Email you when the job starts, ends, or fails |
| `--output`, `--error` | Files for the job's normal output and its error messages. `%J` is the job ID |
| `--partition`, `--account`, `--qos` | Which nodes to use. See [section 7](#7-partitions) |
| `--nodes=1`, `--ntasks=1` | One copy of the program on one node |
| `--cpus-per-task` | How many CPUs the program can use |
| `--mem` | How much RAM the job can use. The job stops if it uses more |
| `--time` | The longest the job can run. `1-0` is one day. Always set it |

### 9.2 Submit the Slurm Script

Submit the job using the `sbatch <slurm_script>` command.

```
sbatch test.sh
```

### 9.3 Cancel a Slurm Job

You can cancel a running job at any moment using 

```
scancel <job_id>
```

If you accidentally run a heavy script on the login node, you can kill the process with Ctrl + C or exit the shell window (this closes the connection).

## 10. Interactive Jobs

You can run an interactive job using the `salloc` command. This is great for shorter tasks, such as testing scripts and debugging. Slurm gives you a shell on a compute node, and you run commands there by hand.

On Hummingbird:

```
salloc --partition=128x24 --mem=10G --ntasks=1 --cpus-per-task=4 --job-name=test
```

On Elkhorn, windfall is especially good for interactive sessions to test code:

```
salloc -p windfall --cpus-per-task 12 --mem=40G --time=4:00:00
```

When the prompt changes to a node name, such as `node-37`, you are on the compute node. Type `exit` to give the node back.

## 11. Checking Job Efficiency

You can see how efficient your Slurm jobs were using the following command: `seff <job_id>`.

Here is an example of one of my jobs from the past (different from the job shown above).

> [!NOTE]
> The `seff` command typically doesn't work while a job is still running.

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

**Memory Utilized.** Array task 150 of Slurm job 398476 only used 26.46 GB of RAM. Next time I run a similar job, I can reduce requested RAM from 40.00 GB to 30.00 GB.

**CPU Efficiency** is the amount of time the requested CPUs were actively doing work relative to the amount of time they were idle. In this case, 80% of the reserved CPU time was actively used for computations, and the remaining ~20% was idle. This is fairly efficient, but next time I could consider requesting slightly fewer CPUs.

> [!TIP]
> CPU usage can vary drastically during the job if you have multiple commands in your Slurm job. Efficiency will be low when only one or a few steps require many CPUs, and the other steps cannot make use of multiple CPUs.

<br/>

# Part III. Software

## 12. Modules

There is are many pre-installed software packages on Hummingbird. Before installing a new software, check to see if it is already available:

```
module avail
```

<details>
<summary><b>Example module avail output</b></summary>

```
---------------------------------------------------------------- /hb/software/modulefiles -----------------------------------------------------------------
   admixture/1.3.0              gcat/1.0                 modkit/0.4.0                    quantumespresso/7.5                 (D)
   alphafold3/3.0.0             gcloud/556.0.0           mosdepth/0.3.4                  raxml-ng/1.2.0
   amber/24                     gdbm/1.26                mrbayes/3.2.7                   repeatmasker/4.1.5
   ant/1.10.10                  gemma/0.98.5             mysql/8.4.8-lts                 rmblast/2.14.1
   antlr/2.7.7                  git/2.52.0        (L)    namd/2.12                       rust/1.79.0
   aria2/1.37.0                 glimpse/2.0.1            ncbi/16.36.0                    rust/1.94.1                         (D)
   aster/1.16                   go/1.22.5                nco/5.3.2                       sambamba/0.8.2
   autoconf/2.73                gradle/8.4               necat/0.0.1                     seqkit/2.5.1
   aws/2.13.15                  guppy/6.4.6-cpu          new-hmmer/3.4                   sfincs/2.1.1-cpu
   bbftp/3.2.1                  guppy/6.4.6-gpu   (D)    nextflow/24.10.4                shapeit5/5.1.1
   bbtools/39.01                hisat/2.1.0              ngsld/1.2.0                     shasta/0.12.0
   beast/2.1.4                  iq-tree/2.2.2.6          nvidia-hpc-sdk/24.7             singularity-ce/singularity-ce.4.1.4
   bedtools/2.26.0              java/8u151               openfoam/OpenFOAM-v2206         slim/4.2.2
   blast/2.17.0                 java/8u471        (D)    orca/5.0.1                      smrtlink/8.0.0
   bwa-mem2/2.2.1               jdk/17.0.7               orca/6.0.1                      spades/4.1.0
   ccache/4.10.2                jdk/21.0.4        (D)    orca/6.1.0                      star/2.7.10b
   cellranger/2.2.0             julia/1.11.0             orca/6.1.1               (D)    structure/2.3.4
   chrome/109.0.5414.119        kmc/3.2.4                paml/4.10.7                     swan/41.45.C
   cuda/12.8.1                  lastz/1.04.00            pandoc/2.14.2                   swan/41.45.Z                        (D)
   cuda/13.1.1           (D)    lastz/1.04.41     (D)    parallel/20200122               trf/4.09.1
   cufflinks/2.2.1              lftp/4.9.3               paraview/5.13.1-gpu             trimgalore/0.6.10
   dorado/0.9.0                 lumpy/0.3.1              paraview/5.13.1-swrender        trimmomatic/0.39
   dorado/0.9.1          (D)    macaulay2/1.25.05        paraview/5.13.3-gpu             udunits/2.2.8
   ecosys/1.1                   mafft/7.520              paraview/5.13.3-swrender (D)    vg/1.12.1
   edirect/062020               magic-blast/1.7.2        perl/5.40.0                     wtdbg2/2.5
   eigensoft/8.0.0              mash/2.3                 pftool/pftool                   xbeach/r6057-mpi
   elai/1.21                    matlab/2023b             phast/1.5                       xbeach/r6057
   fastp/0.23.2                 matlab/2025b      (D)    picard/2.27.1                   xbeach/r6112-mpi
   flye/2.9.2                   mauve/2.4.1              picard/3.4.0             (D)    xbeach/r6112                        (D)
   foldseek/8-ef4e960           migrate/3.6.11           plink/1.90b6.16                 xerces-c/3.3.0
   galprop/57                   miniconda3/3.13   (L)    plink/2.0a7.1            (D)
   gatk/4.4.0.0                 minimap2/2.17            protobuf/28.2
   gaussian/09.D1.01            mitofinder/1.4.2         quantumespresso/7.2

----------------------------------------------------------- /hb/software/moduledeps/miniconda3 ------------------------------------------------------------
   agat/1.2.0                 crpropa/3.2.1                         humann/3.9              ngslca/1.0.5               roary/3.13.0
   alphapulldown/2.5.1        cutadapt/4.4                          hyphy/2.5.73            nwchem/7.2.2               rsem/1.3.3
   angsd/0.940                delly/1.2.6                           hyphy/2.5.79     (D)    odgi/0.8.6                 sage/10.3
   apcluster/1.4.13           demucs/4.0.1                          inla/23.09.09           orthofinder/2.5.5          salmon/1.10.3
   axisem3d/2.1.0             dendropy/4.6.1                        intarna/3.4.0           panaroo/1.3.4              samtools/1.21
   bamdam/0.3.0        (D)    edta/2.2.x                            iq-tree/3.0.1    (D)    panaroo/1.5.1       (D)    seqtk/1.4
   bamdam/24dec14             fastq-screen/0.16.0                   kb-python/0.28.2        pcangsd/1.36.1             shortbred/0.9.4
   bcftools/1.21              fastqc/0.12.1                         kneaddata/0.12.4        pggb/0.6.0                 sideretro/1.1.6
   bcl2fastq2/2.20.0          freeclimber/0.4.0                     kraken/2.1.3            phylophlan/3.0             snakemake/7.32.4
   bifrost/1.1.4              fugassem/0.3.8                        krakenuniq/1.0.4        phyx/1.1.1                 sniffles/2.3.2
   bowtie/2.5.4               geant4/11.3.2                         macs/3.0.1              ppanini/0.7.4              sniffles/2.7.5   (D)
   braker/2.1.6               genomescope/2.1.0                     manta/1.6.0             prokka/1.14.5              stacks/2.65
   busco/5.4.7                gffutils/0.12.0                       metaphlan/4.1.1         prokka/1.15.6       (D)    subread/2.1.1
   buscophylo/1.3             globus-compute-endpoint/4.15.0        metawibele/0.4.7        pyscf/2.7.0                tidyverse/2.0.0
   cactus/2.8.4               gmt/6.4.0                             mitohifi/3.2.2          qiime2/2022.2              toga/1.1.7
   captus/1.6.0               gromacs/2024.2.gpu                    moseq2/1.3.0            qiime2/2024.10      (D)    trinity/2.15.1
   climlab/0.8.2              gromacs/2024.2                 (D)    multiqc/1.27            quast/5.2.0                vcftools/0.1.16
   climlab/0.9.1       (D)    gubbins/3.4                           nanoreviser/1.0         r/4.4.1                    velvet/1.2.10
   coinfinder/1.2.1           heasoft/6.33.2                        newick_utils/1.6        repeatmodeler/2.0.5
   cp2k/2024.1                hifiasm/0.25.0                        nextflow/25.10.4 (D)    rerconverge/0.3.0

---------------------------------------------------------------- /opt/ohpc/pub/modulefiles ----------------------------------------------------------------
   cmake/4.3.2     gnu14/14.2.0    hwloc/2.13.0        ohpc (L)    pmix/4.2.9    ucx/1.20.1
   gnu13/13.2.0    gnu15/15.2.0    libfabric/1.18.0    os          prun/2.2      valgrind/3.27.0

  Where:
   D:  Default Module
   L:  Module is loaded

If the avail list is too long consider trying:

"module --default avail" or "ml -d av" to just list the default modules.
"module overview" or "ml ov" to display the number of modules for each name.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
```

</details>

If you don't see the software you need, you can install it yourself using conda, mamba, pip, or custom GitHub instructions. If you are having trouble installing software, you can submit a ticket by emailing help@ucsc.edu. It is okay to compile software on the login node.

Here is an example command for loading the bcftools module:

```
module load bcftools/1.16
```

## 13. Conda

If the software you want isn't pre-installed on your cluster, conda is a good fallback. Conda is a package and environment manager. It installs a program together with the libraries it needs into a separate *environment*, so tools that need different versions of the same library do not conflict. Miniconda is a small installer that provides conda. On Hummingbird, miniconda is available as a module, so load it first:

```
module load miniconda3
```

Create a new environment using `conda create`.

```
conda create -y -n analysis
```

The `-n` flag specifies the name for the conda environment. The `-y` flag specifies "yes" to all command prompts.

Activate the environment using `conda activate`.

```
conda activate analysis
```

Check if your software is available from Bioconda:

<img width="1014" alt="pbmm2_bioconda" src="https://github.com/user-attachments/assets/b5edb75e-d65d-45c4-9c5a-073389857cf7" />

If so, use their package recipe to install!

```
conda install bioconda::pbmm2
```

You can install many software packages in the same conda environment. Conda handles the conflicts. 

To deactivate the environment, use `conda deactivate`.

<details>
<summary><b>Other useful conda commands</b></summary>

```
# List all environments!
conda info --envs

# Remove a conda environment
conda remove --name <env_name> --all
```

</details>

> [!TIP]
> Run `which <tool>` to see which copy of a program runs. If it shows `~/.local/bin` and not `~/.conda/envs/<env>/bin`, the environment is not active, or a pip install outside conda is in the way.

## 14. Permissions

Imagine you write a quick bash script that prints "Hello World" using the echo command.

```
echo 'echo "Hello World"' > test.sh
```

Trying to run it directly fails:

```
./test.sh
# -bash: ./test.sh: Permission denied
```

To understand why, let's check the permissions for this file using `ls -lah`.

```
ls -lah test.sh
-rw-r--r-- 1 matt staff 19 Jul  7 16:44 test.sh
```

Permissions are organized by owner, group, others, in the format of read (r), write (w), execute (x). The first character is the file type. The next nine characters are three groups of three, one group for each kind of user. Within each group the order is always read, write, execute, and a `-` means that permission is off.

```
 -    rw-    r--    r--
 │     │      │      └─ everyone else:  read only
 │     │      └──────── group:          read only
 │     └─────────────── owner (you):    read and write, no execute
 └───────────────────── file type:      - is a file, d is a directory
```

This file does not have executable permissions. Let's change that using `chmod`. In `a+x`, the `a` means all users (owner, group and everyone else), the `+` adds a permission, and the `x` is execute. `u+x` would add execute for the owner only, and `a-w` would remove write for everyone.

```
chmod a+x test.sh
ls -lah test.sh
-rwxr-xr-x 1 matt staff 19 Jul  7 16:44 test.sh
```

Now it is executable and will run!

```
./test.sh
Hello World
```

<br/>

# Part IV. Scaling Up

## 15. Array Jobs

An array job runs the same script many times in parallel, once for each input. Slurm calls each copy a *task* and gives it its own number in the variable `$SLURM_ARRAY_TASK_ID`. Your script uses that number to pick its input.

This is useful when you need to run the same analysis (e.g., `samtools view`) on many files or samples. Instead of writing 30 separate Slurm scripts, or one Slurm script with a for loop, you submit one script with `--array=0-29` and let Slurm run the tasks in parallel.

The examples below use the lab-colibri partition on Elkhorn.

### 15.1 A Worked Example in Bash

For example, let's say I have 33 mapped BAM files that I want to filter to only include primary alignments from chromosome 6 with MAPQ > 30.

```
pwd
```

```
/scratch/mglasena/hb_tutorial
```

```
ls -lah *.bam
```

<details>
<summary><b>The 33 BAM files</b></summary>

```
-rw-r--r-- 1 mglasena ucsc_p_all_usr 401M Jul 29 11:34 HG002.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 293M Jul 29 11:34 HG003.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 213M Jul 29 11:34 HG004.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 192M Jul 29 11:34 HG005.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 347M Jul 29 11:34 HG01106.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 362M Jul 29 11:34 HG01258.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 582K Jul 29 11:34 HG01891.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 297M Jul 29 11:34 HG01928.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 246M Jul 29 11:34 HG02055.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 240M Jul 29 11:34 HG02630.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 173M Jul 29 11:34 HG03492.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  99M Jul 29 11:34 HG03579.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  68M Jul 29 11:34 IHW09021.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 134M Jul 29 11:34 IHW09049.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  44M Jul 29 11:34 IHW09071.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 151M Jul 29 11:34 IHW09117.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  29M Jul 29 11:34 IHW09118.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 217M Jul 29 11:34 IHW09122.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 125M Jul 29 11:34 IHW09125.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  65M Jul 29 11:34 IHW09175.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 100M Jul 29 11:34 IHW09198.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 116M Jul 29 11:34 IHW09200.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 115M Jul 29 11:34 IHW09224.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  14M Jul 29 11:34 IHW09245.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  68M Jul 29 11:34 IHW09251.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 176M Jul 29 11:34 IHW09359.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  33M Jul 29 11:34 IHW09364.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr  76M Jul 29 11:34 IHW09409.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 135M Jul 29 11:34 NA19240.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 336M Jul 29 11:34 NA20129.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 292M Jul 29 11:34 NA21309.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 366M Jul 29 11:34 NA24694.dedup.trimmed.hg38.bam
-rw-r--r-- 1 mglasena ucsc_p_all_usr 148M Jul 29 11:34 NA24695.dedup.trimmed.hg38.bam
```

</details>

If I want to use samtools view to filter each file, I have a few options:

1. Create 33 different Slurm scripts and submit them one by one (tedious).
2. Create one Slurm script with an iteration statement (for loop) that filters each in succession (slow, and the entire job crashes after the first error).
3. Create one array job to process them in parallel.

In the array job, each task (one sample, one file) has its own stderr and stdout, and any errors thrown don't affect the other tasks or cause the parent job to fail. Assuming I have access to the compute resources needed to filter all 33 BAM files in parallel, the array job finishes 33X faster than using a for loop. This speedup is critical, because tomorrow, my mentor might change their mind and say that we need to filter at MAPQ > 40 instead of MAPQ > 30.

Here is how I would code an array job to process these samples. The whole job is one Slurm script, with no separate program.

```bash
#!/bin/bash
#SBATCH --job-name=samtools
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<cruzid>@ucsc.edu
#SBATCH --output=samtools_%A_%a.out
#SBATCH --error=samtools_%A_%a.err
#SBATCH --mem=4G
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --time=00:10:00
#SBATCH --array=0-32
#SBATCH --partition=lab-colibri
#SBATCH --qos=pi-jkoc
#SBATCH --account=pi-jkoc

# Define array variable containing the paths to all deduplicated BAMs
bam_files=(
	/scratch/mglasena/hb_tutorial/HG002.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG003.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG004.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG005.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG01106.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG01258.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG01891.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG01928.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG02055.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG02630.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG03492.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/HG03579.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09021.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09049.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09071.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09117.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09118.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09122.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09125.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09175.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09198.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09200.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09224.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09245.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09251.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09359.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09364.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/IHW09409.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/NA19240.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/NA20129.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/NA21309.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/NA24694.dedup.trimmed.hg38.bam
	/scratch/mglasena/hb_tutorial/NA24695.dedup.trimmed.hg38.bam
)

# Define input and output files by indexing the bam_files variable
input_bam=${bam_files[$SLURM_ARRAY_TASK_ID]}
output_bam=${input_bam%.bam}.chr6.primary.mapq30.bam

echo "SLURM_ARRAY_TASK_ID=$SLURM_ARRAY_TASK_ID"
echo "Input BAM: $input_bam"
echo "Output BAM: $output_bam"

# Run samtools view command
# -F 2304 excludes secondary and supplementary alignments
# Check out https://samformat.pages.dev/sam-format-flag
samtools index "$input_bam"
samtools view -b -q 30 -F 2304 "$input_bam" chr6 > "$output_bam"
```

> [!IMPORTANT]
> Make sure `--array=0-N` in your Slurm script matches the number of items you're looping over. With 0-based indexing, 33 files need `--array=0-32`.

A few notes:

1. You can simplify this by writing code to scrape the files from a specified directory instead of listing them all in the script.
2. It's helpful to add print statements (e.g., `echo "Input BAM: $input_bam"`) for debugging purposes.
3. The header flags `#SBATCH --output=samtools_%A_%a.out` and `#SBATCH --error=samtools_%A_%a.err` tell Slurm to create separate output and error files for each task, named `samtools_<job_id>_<array_task_id>`.

### 15.2 Picking the Input in Python

The task number can also go to a Python script. Slurm puts `SLURM_ARRAY_TASK_ID` in the environment of every task, and Python reads it with the `os` module.

```
#!/bin/bash
#SBATCH --job-name=process_samples
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<cruzid>@ucsc.edu
#SBATCH --output=process_samples_%A_%a.out
#SBATCH --error=process_samples_%A_%a.err
#SBATCH --mem=40G
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=6
#SBATCH --time=3-0
#SBATCH --array=0-99
#SBATCH --partition=lab-colibri
#SBATCH --qos=pi-jkoc
#SBATCH --account=pi-jkoc

# -u prints the script's output to the .out file as it runs, not only at the end
python3 -u process_samples.py
```

In the Python script, the task number picks one item from a list or dictionary. If I had a dictionary of samples to process, I only need the number of samples to match `--array` in the Slurm script.

```python
import os

# Slurm sets this variable for each task: 0 for the first task, 1 for the second, and so on.
# It is text, so convert it to an integer.
array_id = int(os.environ["SLURM_ARRAY_TASK_ID"])

# One entry per sample. 100 samples here, so the Slurm script uses --array=0-99.
samples = {
    "sample1": ["data"],
    "sample2": ["data"],
    # ...
}

# Task 0 gets the first sample, task 1 the second, and so on.
sample_name = list(samples.keys())[array_id]
print(f"Processing {sample_name}")
```

### 15.3 Scripting in Python

I don't like writing code in bash because the syntax is not very intuitive or human readable. You can write the whole workflow in Python and use the Slurm script only to run it. Here is the samtools job from [section 15.1](#151-a-worked-example-in-bash), with the work moved into a Python script.

```bash
#!/bin/bash
#SBATCH --job-name=samtools
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<cruzid>@ucsc.edu
#SBATCH --output=samtools_%A_%a.out
#SBATCH --error=samtools_%A_%a.err
#SBATCH --mem=4G
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --time=00:10:00
#SBATCH --array=0-32
#SBATCH --partition=lab-colibri
#SBATCH --qos=pi-jkoc
#SBATCH --account=pi-jkoc

python3 -u filter_bam.py
```

Each array task runs `python3 filter_bam.py`. Here is the script.

<details>
<summary><b>filter_bam.py</b></summary>

```python
import os
import subprocess

# The 33 input BAM files. Task N processes the file at position N (0-based).
bam_files = [
	"/scratch/mglasena/hb_tutorial/HG002.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG003.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG004.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG005.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG01106.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG01258.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG01891.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG01928.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG02055.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG02630.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG03492.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/HG03579.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09021.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09049.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09071.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09117.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09118.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09122.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09125.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09175.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09198.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09200.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09224.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09245.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09251.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09359.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09364.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/IHW09409.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/NA19240.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/NA20129.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/NA21309.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/NA24694.dedup.trimmed.hg38.bam",
	"/scratch/mglasena/hb_tutorial/NA24695.dedup.trimmed.hg38.bam"
]


def index_bam(bam_file):
	# samtools needs an index (.bai) to read one chromosome from a BAM
	index_command = "samtools index {bam}".format(bam = bam_file)
	# check=True stops the script with an error if samtools fails.
	# shell=True runs the command through the shell, as if you typed it.
	subprocess.run(index_command, check=True, shell=True)

def filter_bam(bam_file):
	# Name the output after the input, e.g. HG002...bam -> HG002...chr6.primary.mapq30.bam
	output_bam = bam_file.replace(".bam", ".chr6.primary.mapq30.bam")

	# -q 30 keeps reads with MAPQ >= 30, -F 2304 drops secondary and supplementary
	# alignments, and chr6 keeps only reads on chromosome 6
	samtools_command = "samtools view -b -q 30 -F 2304 {input_bam} chr6 > {output_bam}".format(input_bam = bam_file, output_bam = output_bam)

	subprocess.run(samtools_command, check=True, shell=True)

def main():
	# Slurm sets SLURM_ARRAY_TASK_ID for each task (0 to 32 here)
	array_id = int(os.environ["SLURM_ARRAY_TASK_ID"])
	print("Array ID: {}".format(array_id))

	# Pick this task's file
	bam_file = bam_files[array_id]
	print("Processing {file}".format(file = bam_file))

	index_bam(bam_file)
	filter_bam(bam_file)

# Run main() only when the file is run as a script, not when it is imported
if __name__ == "__main__":
	main()
```

</details>

Here, I have neatly organized the different steps into functions that are called by `main()`. I highly recommend using Python to organize the different steps of a workflow into functions.

## 16. Elkhorn Daily Tips

### 16.1 Requeuing

A windfall job on another lab's node is canceled when that lab needs the node. To have Slurm put the job back in the queue and run it again, add the requeue line to the header and source the requeue script at the start of your job.

```bash
# add SBATCH requeue line
#SBATCH --requeue

# source requeue script at the beginning of your executable
source /software/scripts/utility/requeue.feature
```

### 16.2 Serial Jobs

Some work runs in steps, where step 2 needs the output of step 1. Do not submit step 2 and hope that step 1 has finished. Tell Slurm to start step 2 only when step 1 finishes with no error.

```bash
# submit_pipeline.sh. Run it on the login node with: bash submit_pipeline.sh
step1=$(sbatch --parsable step1_align.sh)
step2=$(sbatch --parsable --dependency=afterok:$step1 --kill-on-invalid-dep=yes step2_call.sh)
sbatch --dependency=afterok:$step2 --kill-on-invalid-dep=yes step3_summarize.sh
```

`--parsable` makes `sbatch` print only the job ID, so the script can save it. `--dependency=afterok:$step1` holds step 2 until step 1 ends with exit code 0. While it waits, `squeue` shows `(Dependency)` in the reason column.

If step 1 fails, step 2 can never start. `--kill-on-invalid-dep=yes` cancels step 2 at once. Without it, step 2 stays in the queue with the reason `DependencyNeverSatisfied` until you cancel it.

> [!IMPORTANT]
> Slurm only knows that a step failed if the step script returns an error. Put `set -euo pipefail` near the top of each step script. Then any failed command, also inside a pipe, stops the script with an error, and the next step does not start.

<details>
<summary><b>Other dependency types</b></summary>

| Dependency | The next job starts when |
|---|---|
| `afterok:<job_id>` | The job finished with no error |
| `afterany:<job_id>` | The job finished, with or without an error |
| `afternotok:<job_id>` | The job failed. Use this for a cleanup or alert job |
| `afterok:<array_job_id>` | All tasks of the array finished with no error |
| `aftercorr:<array_job_id>` | The same task number of the earlier array finished with no error. Task 5 of step 2 waits only for task 5 of step 1 |

You can list more than one job, for example `--dependency=afterok:1234:1235`.

</details>

On windfall, a job that is canceled for the node owners did not finish with no error. With `--requeue` ([section 16.1](#161-requeuing)), the job keeps its job ID and runs again, and the next step keeps waiting for it.

### 16.3 Downloading Large Files

Downloads are limited by the network card, so distribute them across machines rather than cores. Slurm silently packs array tasks onto one node without `--exclusive`.

<details>
<summary><b>More detail</b></summary>

- 17 array tasks with 4 CPUs each can all land on one 112-core node. Add `#SBATCH --exclusive` to get one task per node. Check where tasks run with `squeue -u <your_cruzid> -o '%.14i %.8T %M %N'`.
- Compute nodes have a 1 Gb network link to the outside. A single download stream gets about 45 MB/s, so 3 or 4 streams fill one node.
- Nodes 29 and 33 to 36 have a slower 100 Mb network link. For downloads, add `#SBATCH --exclude=node-29,node-33,node-34,node-35,node-36`.

</details>

### 16.4 Filesystem

The cluster's shared filesystem has a fixed total I/O bandwidth, a ceiling on how much data it can read and write per second across all users. Every job that reads or writes files takes a share, so the speed any one job sees depends on how many other jobs hit the filesystem at the same time. Running more jobs in parallel spreads the same bandwidth thinner.

## 17. Troubleshooting

<details>
<summary><b>My job stays in the queue (PD) for a long time</b></summary>

Look at the `NODELIST(REASON)` column of `squeue -u <your_cruzid>`. `Resources` means the nodes are busy, so wait or ask for fewer CPUs or less memory. `Priority` means other jobs are ahead of yours. If you asked for more memory or CPUs than any node has, the job never starts. Cancel it and ask for less.

</details>

<details>
<summary><b>My job ended early</b></summary>

Run `seff <job_id>` and read the `.err` file. `OUT_OF_MEMORY` means the job used more than `--mem`, so ask for more. `TIMEOUT` means it hit `--time`. On windfall, the job may have been stopped for the node owners. See [section 16.1](#161-requeuing).

</details>

<details>
<summary><b><code>command not found</code> in a job, but the command works when I type it</b></summary>

Your job does not see the same environment as your login shell. Load the module or activate the conda environment inside the script, before the command.

</details>

<details>
<summary><b><code>Permission denied</code> when I run a script</b></summary>

The file is not executable. See [section 14](#14-permissions). Or run it with `bash script.sh`.

</details>

<details>
<summary><b><code>No such file or directory</code> for a path that I know exists</b></summary>

Check that you use `/scratch/<your_cruzid>`, not `/hb/scratch`. `/hb/scratch` exists only on the login nodes. Also, `/data/colibri` is not mounted on the windfall nodes. Check a path on a compute node with `ls` inside an `salloc` session.

</details>

<details>
<summary><b>All my array tasks run on one node</b></summary>

Slurm packs tasks onto one node when they fit. That is fine for most jobs. If each task needs its own node, add `#SBATCH --exclusive`. See [section 16.2](#163-downloading-large-files).

</details>

<br/>

# Cheat Sheet

| Task | Command |
|---|---|
| Log in to Hummingbird (VPN if off campus) | `ssh <your_cruzid>@hb.ucsc.edu` |
| Log in to Elkhorn | `ssh <your_cruzid>@elkhorn.ucsc.edu` |
| Go to scratch | `cd /scratch/<your_cruzid>` |
| See the partitions | `sinfo` |
| See your jobs | `squeue -u <your_cruzid>` |
| See one job in detail | `scontrol show job <job_id>` |
| Submit a job | `sbatch my_job.sh` |
| Cancel a job | `scancel <job_id>` |
| Get an interactive node | `salloc -p windfall --cpus-per-task 12 --mem=40G --time=4:00:00` on Elkhorn |
| Check a finished job | `seff <job_id>` |
| Find installed software | `module avail` |
| Make a script executable | `chmod a+x script.sh` |
