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

Learn more about Slurm at:

1. Hummingbird homepage, https://hummingbird.ucsc.edu/, and the Hummingbird Getting Started page, https://hummingbird.ucsc.edu/getting-started/
2. Stanford Slurm tutorial: https://login.scg.stanford.edu/tutorials/job_scripts/
3. ChatGPT

If you are a regular user, you should join the Hummingbird Slack community: https://ucschummingbi-lph3072.slack.com/join/shared_invite/zt-19mbwqvx1-GqguQcumVBLss~nzjOHAYg#/shared-invite/email

The Hummingbird team also has weekly office hours on Thursday from 1:00 PM to 2:00 PM via Zoom: https://hummingbird.ucsc.edu/documentation/hummingbird-open-office-hour/

You can also put in a ticket by emailing hummingbird@ucsc.edu.

You can monitor cluster usage at https://hummingbird.ucsc.edu/current-usage/

There are example Slurm scripts on Hummingbird located at `/hb/software/scripts`.

## 2. What Is a Cluster?

A cluster, or supercomputer, is a group of computers that work together and function as a single system. The advantage of using Hummingbird is that it has much more memory and storage than your own personal computers. We can submit jobs to run on Hummingbird and will be notified by email when they complete. We do not have to keep our computer running or monitor the progress.

We interact with Hummingbird via the command line interface using UNIX commands. UNIX is an operating system that includes a collection of built-in tools and commands. Here is a great command line tutorial if you have no prior experience: https://www.codecademy.com/learn/learn-the-command-line.

The commands used to submit and monitor jobs on Hummingbird are mostly not Hummingbird-specific. Hummingbird uses the Slurm workload manager software. Slurm (Simple Linux Utility for Resource Management) is a job scheduler that automates the process of allocating resources (i.e., hardware) for users' computational tasks. There is tons of content on Slurm on the internet.

<p align="center">
  <img src="https://github.com/user-attachments/assets/cdbfeb86-011c-4b89-85cf-e19cf8bf67e4" alt="compute_cluster" width="700">
</p>

Every cluster has two kinds of computers. You log in to the **login node**. The work runs on the **compute nodes**. Slurm decides which compute node runs your job.

> [!WARNING]
> Do not run code or scripts on the login node. It slows it down for all users. Use `sbatch` or `salloc`, which are covered in [Part II](#part-ii-running-jobs).

## 3. Hummingbird and Elkhorn

UCSC has two Slurm systems that you will use. They work the same way. They have different nodes and different partitions.

| | Hummingbird | Elkhorn |
|---|---|---|
| What it is | The campus cluster | The system for PI-owned nodes |
| Log in with | `ssh <your_cruzid>@hb.ucsc.edu` | `ssh <your_cruzid>@elkhorn.ucsc.edu` |
| Our lab's nodes | | node-38 and node-39 (lab-colibri), node-40 (lab-colibri-hmem) |
| Shared queue | 128x24 and other partitions | windfall, which spans every node on Elkhorn |
| Node names | `node-XX` | `node-XX` |
| Home | `/home/<your_cruzid>` | The same directory |
| Scratch | `/scratch/<your_cruzid>` | The same directory |

Both systems share your home and scratch directories, so a file you write on one is there on the other.

Use Elkhorn for work on our lab's nodes, and use windfall when you are stuck in a queue. Most of Elkhorn is idle most of the time. Use Hummingbird for the campus partitions.

Access to Elkhorn requires your PI to have sponsored you. You can check whether you are linked to the pi-jkoc lab account with the following command:

```
sacctmgr show assoc user=$USER format=cluster,account,partition,qos%30,share
```

## 4. Logging In

If not on the campus WiFi, you will need to be connected to the campus VPN.

To log in, first open the terminal application (Mac users) or PuTTY (Windows users). We will use the ssh command, which stands for secure shell and provides a secure connection between your computer and the server. To log in to Hummingbird, you will use the following command:

```
ssh <your_cruzid>@hb.ucsc.edu
```

Replace `<your_cruzid>` with your UCSC username in your command. You will be prompted to enter your password. For security measures, you will not be able to see the characters you are entering. Type your password and press enter.

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

After running this command, `/home/<your_cruzid>` will be printed to the terminal. This is your personal home directory, where you can store files and data. Hummingbird users have a 1 TB storage quota in their home directory. I recommend using your home directory for longer-term storage, including stable files and software installs.

You'll want to work mostly in your scratch directory, at `/scratch/<your_cruzid>`, where there is no storage quota. This is a great location to test new scripts and to store intermediate files.

```
cd /scratch/<your_cruzid>
```

| Location | Use it for | Notes |
|---|---|---|
| `/home/<your_cruzid>` | Software installs, scripts, files you keep | 1 TB quota |
| `/scratch/<your_cruzid>` | Running jobs and intermediate files | Not backed up. Old files may be deleted, see the [data and backup policy](https://hummingbird.sites.ucsc.edu/documentation/hummingbird-data-storage-and-backup-policy/) |

> [!WARNING]
> Always write `/scratch/<your_cruzid>` in your scripts. `/hb/scratch` also works on the login nodes, but it does not exist on the compute nodes, so a job that uses it fails.

Work in your scratch directory by default. Ask your PI where to store important files.

> [!CAUTION]
> Scratch is not backed up. Keep a copy of anything you cannot regenerate in your home directory or on your own computer.

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

Let's learn more about the cluster.

```
sinfo
```

The different partitions are listed. Partitions are groups of nodes with different configurations. The `STATE` column shows whether a node is free (`idle`), partly used (`mix`), full (`alloc`), or being taken out of service (`drng`).

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
| `lab-colibri` | node-38, node-39 | Our lab | `--partition=lab-colibri --account=pi-jkoc --qos=pi-jkoc` |
| `lab-colibri-hmem` | node-40 | Our lab | `--partition=lab-colibri-hmem --account=pi-jkoc --qos=pi-jkoc` |
| `windfall` | every node | Everyone on Elkhorn | `--partition=windfall` |
| `512x64` | node-29, node-30 | Another lab's nodes | Reach them through windfall |

#### Colibri

Our lab has some private nodes with increased CPU and RAM. We have two nodes on the partition `lab-colibri` (node-38, node-39) and one high memory node on the partition `lab-colibri-hmem` (node-40). node-38 and node-39 have 112 CPUs and 1000 GB RAM each. node-40 is a high memory node with 112 CPUs and 2000 GB RAM.

> [!IMPORTANT]
> There are no hard-coded CPU limits on the lab-colibri and lab-colibri-hmem partitions. Please be considerate of how much compute resources you are allocating at a single time. If the nodes are idle and you have a high priority job, feel free to allocate a lot of resources. If there are many people in the queue and your job is not time-sensitive, try to occupy fewer CPUs to share the resource!

Because the lab-colibri partition does not have a hard-coded CPU limit, it will run as many array tasks as possible given the available CPU and RAM. You can set the number of array tasks that run at the same time like this:

```
#SBATCH --array=0-999%10
```

This says create 1,000 array tasks (0-based indexing), but only run 10 tasks at a time. [Section 15](#15-array-jobs) covers array jobs.

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

In addition to our lab's nodes, we also have access to other lab groups' nodes when they're idle! The windfall partition (`--partition=windfall`) spans every node in the cluster, and it has no CPU limit. It is the default partition on Elkhorn (the `*` in `sinfo`).

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
<summary><b>Example squeue output from Hummingbird (2025)</b></summary>

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

</details>

Check the status of a specific user's jobs (queued and running) with `squeue -u <username>`.

```
squeue -u mglasena
```

```
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            407851    128x24 genotype mglasena  R 5-21:24:31      1 hbnode-17
```

My job (id 407851, name genotype) has been running for 5 days, 21 hours, 24 minutes, and 31 seconds on hbnode-17, which is part of the 128x24 partition. Let's get more detail about this job using `scontrol show job <job_id>`.

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

Let's use the nano text editor to write a Slurm script.

```
nano test.sh
```

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

<details>
<summary><b>What each header line means</b></summary>

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

</details>

### 9.2 Submit the Slurm Script

Submit the job using the `sbatch <slurm_script>` command.

```
sbatch test.sh
```

### 9.3 Cancel a Slurm Job

You can cancel a running job at any moment using `scancel <job_id>`.

If you accidentally run a job on the login node by mistake, you can kill the process with Ctrl + C or exit the shell window (this closes the connection).

## 10. Interactive Jobs

You can run an interactive job using the `salloc` command. This is great for debugging. Slurm gives you a shell on a compute node, and you run commands there by hand.

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

Let's inspect one of my recent jobs. This is a different job from the one above. Note that the `seff` command typically doesn't work while a job is still running.

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

**CPU Efficiency** is the amount of time the requested CPUs were actively doing work relative to the amount of time they were idle. In this case, 80% of the reserved CPU time was actively used for computations, and the remaining ~20% was idle. This is fairly efficient, but next time I could consider requesting slightly fewer CPUs. Note that CPU usage can vary drastically during the job if you have multiple commands in your Slurm job. Efficiency will be low when only one or a few steps require many CPUs, and the other steps cannot make use of multiple CPUs.

<br/>

# Part III. Software

## 12. Modules

There is a lot of pre-installed software on Hummingbird. Before installing a new software, check to see if it is already available:

```
module avail
```

<details>
<summary><b>Example module avail output</b></summary>

```
--------------------------------------------------- /hb/software/moduledeps/spack ---------------------------------------------------
   adios2/2.9.2                 cfitsio/4.3.0    htslib/1.16                 opencv/4.8.0                  r/4.3.0
   angsd/0.935                  cp2k/2023.2      htslib/1.17          (D)    openmpi/4.1.6                 root/6.24.06
   armadillo/12.4.0             delly2/1.1.6     jags/4.3.0                  parallel-netcdf/1.12.3        salmon/1.10.2
   bcftools/1.16                fftw/3.3.10      jellyfish/2.2.7             pcre/8.45                     samtools/1.16.1
   bcl2fastq2/2.20.0.422 (D)    gate/9.1         lammps/20230802             pcre2/10.42                   samtools/1.17
   beagle/5.4                   gdal/3.7.3       libpng/1.6.39               perl/5.32.1                   sqlite/3.43.2
   berkeley-db/18.1.40          geant4/10.7.4    maker/3.01.04               perl/5.38.0                   sratoolkit/3.0.0
   boost/1.55.0                 geos/3.12.0      netcdf-c/4.9.2              phyx/1.3.1             (D)    stacks/2.53
   boost/1.83.0          (D)    gmake/4.3        netcdf-fortran/4.6.1        pmix/5.0.1             (D)    stringtie/2.2.1
   bowtie/1.3.1-7               gsl/2.7.1        nlopt/2.7.0                 proj/9.2.1                    swig/4.1.1
   bowtie2/2.5.1                hdf5/1.14.0      nwchem/7.2.0                psmc/2016-1-21                trimmomatic/0.39
   bwa/0.7.17                   hmmer/3.4        ont-guppy/6.1.7             qualimap/2.2.1                vcftools/0.1.16  (D)

----------------------------------------------------- /hb/software/modulefiles ------------------------------------------------------
   admixture/1.3.0              galprop/57               mitofinder/1.4.2                repeatmasker/4.1.5
   amber/24                     gatk/4.4.0.0             modkit/0.4.0                    rmblast/2.14.1
   ant/1.10.10                  gaussian/09.D1.01        mosdepth/0.3.4                  rust/1.79.0
   antlr/2.7.7                  gcat/1.0                 namd/2.12                       sambamba/0.8.2
   aster/1.16                   gemma/0.98.5             ncbi/16.36.0                    seqkit/2.5.1
   aws/2.13.15                  go/1.22.5                nco/5.3.2                       sfincs/2.1.1-cpu
   bbftp/3.2.1                  gradle/8.4               necat/0.0.1                     shasta/0.12.0
   bbtools/39.01                guppy/6.4.6-cpu          new-hmmer/3.4                   singularity-ce/singularity-ce.4.1.4
   beast/2.1.4                  guppy/6.4.6-gpu   (D)    nextflow/24.10.4                slim/4.2.2
   bedtools/2.26.0              hisat/2.1.0              ngsld/1.2.0                     smrtlink/8.0.0
   blast/2.15.0                 iq-tree/2.2.2.6          nvidia-hpc-sdk/24.7             spades/4.1.0
   bwa-mem2/2.2.1               java/8u151               openfoam/OpenFOAM-v2206         star/2.7.10b
   ccache/4.10.2                jdk/17.0.7               orca/5.0.1                      structure/2.3.4
   cellranger/2.2.0             jdk/21.0.4        (D)    orca/6.0.1                      swan/41.45.C
   chrome/109.0.5414.119        julia/1.11.0             orca/6.1.0               (D)    swan/41.45.Z                        (D)
   cuda/12.5.1                  lastz/1.04.00            paml/4.10.7                     trf/4.09.1
   cuda/12.6             (D)    lastz/1.04.41     (D)    pandoc/2.14.2                   trimgalore/0.6.10
   cufflinks/2.2.1              lftp/4.9.3               parallel/20200122               udunits/2.2.8
   dorado/0.9.0                 lumpy/0.3.1              paraview/5.13.1-gpu             vg/1.12.1
   dorado/0.9.1          (D)    mafft/7.520              paraview/5.13.1-swrender (D)    wtdbg2/2.5
   ecosys/1.1                   magic-blast/1.7.2        perl/5.40.0              (D)    xbeach/r6057-mpi
   edirect/062020               mash/2.3                 phast/1.5                       xbeach/r6057
   eigensoft/8.0.0              matlab/2021b             picard/2.27.1                   xbeach/r6112-mpi
   elai/1.21                    matlab/2023b      (D)    plink/1.09                      xbeach/r6112                        (D)
   fastp/0.23.2                 migrate/3.6.11           protobuf/28.2                   xerces-c/3.3.0
   flye/2.9.2                   miniconda3/3.12          quantumespresso/7.2
   foldseek/8-ef4e960           minimap2/2.17            raxml-ng/1.2.0

------------------------------------------------ /hb/software/moduledeps/miniconda3 -------------------------------------------------
   agat/1.2.0               crpropa/3.2.1              hyphy/2.5.50            ngslca/1.0.5               rerconverge/0.3.0
   angsd/0.940       (D)    cutadapt/4.4               hyphy/2.5.73     (D)    nwchem/7.2.2        (D)    roary/3.13.0
   apcluster/1.4.13         delly/1.2.6                inla/23.09.09           odgi/0.8.6                 sage/10.3
   bamdam/24dec14           demucs/4.0.1               intarna/3.4.0           orthofinder/2.5.5          salmon/1.10.3     (D)
   bcftools/1.21     (D)    dendropy/4.6.1             kb-python/0.28.2        panaroo/1.3.4              samtools/1.21     (D)
   bcl2fastq2/2.20.0        edta/2.2.x                 kraken/2.1.3            panaroo/1.5.1       (D)    seqtk/1.4
   bifrost/1.1.4            fastq-screen/0.16.0        krakenuniq/1.0.4        pcangsd/1.36.1             snakemake/7.32.4
   bowtie/2.5.4      (D)    fastqc/0.12.1              macs/3.0.1              pggb/0.6.0                 sniffles/2.3.2
   braker/2.1.6             freeclimber/0.4.0          manta/1.6.0             phyx/1.1.1                 stacks/2.65       (D)
   busco/5.4.7              gffutils/0.12.0            metaphlan/4.1.1         prokka/1.14.5              tidyverse/2.0.0
   buscophylo/1.3           gmt/6.4.0                  metawibele/0.4.7        pyscf/2.7.0                toga/1.1.7
   cactus/2.8.4             gromacs/2024.2.gpu         mitohifi/3.2.2          qiime2/2022.2              trinity/2.15.1
   climlab/0.8.2            gromacs/2024.2      (D)    moseq2/1.3.0            qiime2/2024.10      (D)    vcftools/0.1.16
   climlab/0.9.1     (D)    gubbins/3.4                multiqc/1.27            quast/5.2.0
   coinfinder/1.2.1         heasoft/6.33.2             nanoreviser/1.0         r/4.4.1             (D)
   cp2k/2024.1       (D)    humann/3.9                 newick_utils/1.6        repeatmodeler/2.0.5

----------------------------------------------------- /opt/ohpc/pub/modulefiles -----------------------------------------------------
   autotools       gnu13/13.2.0      intel/2024.0.0   (D)    ohpc       (L)    pmix/4.2.9      ucx/1.17.0
   cmake/3.24.2    hwloc/2.11.1      intel/2024.2.0          os                prun/2.2        valgrind/3.23.0
   gnu12/12.2.0    intel/2023.2.1    libfabric/1.18.0        papi/6.0.0        spack/0.22.2

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

If you don't see the software you need, you can install it yourself using conda, mamba, pip, etc. If you are having trouble installing it, you can submit a ticket by emailing hummingbird@ucsc.edu. It is okay to compile software on the login node.

Here is an example command for loading the bcftools module:

```
module load bcftools/1.16
```

## 13. Conda

If the software you want isn't pre-installed on your cluster, conda is a good fallback. On Hummingbird, load miniconda first:

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

Say you write a quick bash script that prints "Hello World" using the echo command.

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

Permissions are organized by owner, group, others, in the format of read (r), write (w), execute (x).

<details>
<summary><b>How to read <code>-rw-r--r--</code></b></summary>

| Characters | Who | Meaning here |
|---|---|---|
| `-` | | A normal file. `d` means a directory |
| `rw-` | Owner (you) | Read and write, no execute |
| `r--` | Group | Read only |
| `r--` | Everyone else | Read only |

</details>

This file does not have executable permissions. Let's change that using `chmod`.

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

Array jobs are a powerful feature of Slurm that let you run many similar jobs in parallel, each with a different input or task index. Instead of writing and submitting 30 separate Slurm scripts or running one Slurm script with a for loop, you can submit one script with `--array=0-29` and let Slurm manage the parallelism.

This is especially useful when you need to run the same analysis (e.g., samtools view, python3 script.py) across multiple input files or samples.

The examples below use the lab-colibri partition on Elkhorn.

### 15.1 The Basic Pattern

Each array task gets its own number in `$SLURM_ARRAY_TASK_ID`. Your script uses that number to pick its input.

```
#!/bin/bash
#SBATCH --job-name=process_ont
#SBATCH --mail-type=ALL
#SBATCH --mail-user=<cruzid>@ucsc.edu
#SBATCH --output=process_ont_%A_%a.out
#SBATCH --error=process_ont_%A_%a.err
#SBATCH --mem=40G
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=6
#SBATCH --time=3-0
#SBATCH --array=0-99
#SBATCH --partition=lab-colibri
#SBATCH --qos=pi-jkoc
#SBATCH --account=pi-jkoc

array_id=$SLURM_ARRAY_TASK_ID
export array_id

python3 -u process_ont.py
```

In your Python script, you can extract the array ID (integer) and use it to index a list, dictionary, etc. If I had a dictionary of samples to process, I just have to specify the total number of samples I have in the Slurm script.

```python
import os

array_id = int(os.environ["array_id"])

samples = {
    "sample1": ["data"],
    "sample2": ["data"],
    # ...
}

sample_name = list(samples.keys())[array_id]
print(f"Processing {sample_name}")
```

> [!IMPORTANT]
> Make sure `--array=0-N` in your Slurm script matches the number of items you're looping over. With 0-based indexing, 100 samples need `--array=0-99`.

### 15.2 Worked Example

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

Here is how I would code an array job to process these samples:

<details>
<summary><b>samtools array job in bash</b></summary>

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

</details>

A few notes:

1. You can simplify this by writing code to scrape the files from a specified directory instead of listing them all in the script.
2. It's helpful to add print statements (e.g., `echo "Input BAM: $input_bam"`) for debugging purposes.
3. The header flags `#SBATCH --output=samtools_%A_%a.out` and `#SBATCH --error=samtools_%A_%a.err` tell Slurm to create separate output and error files for each task, named `samtools_<job_id>_<array_task_id>`.

### 15.3 Array Jobs in Python

I don't like writing code in bash because the syntax is not very intuitive or human readable. When I cook up my own array jobs, I write a Python script to be executed inside the Slurm script.

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

Here, each array task will execute `python3 filter_bam.py`. I include the script below.

<details>
<summary><b>filter_bam.py</b></summary>

```python
import os
import subprocess

working_directory = "/scratch/mglasena/hb_tutorial/"

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
	index_command = "samtools index {bam}".format(bam = bam_file)
	subprocess.run(index_command, check=True, shell=True)

def filter_bam(bam_file):
	# Define output bam file name
	output_bam = bam_file.replace(".bam", ".chr6.primary.mapq30.bam")

	# Define samtools view command
	samtools_command = "samtools view -b -q 30 -F 2304 {input_bam} chr6 > {output_bam}".format(input_bam = bam_file, output_bam = output_bam)

	# Execute samtools view command
	subprocess.run(samtools_command, check=True, shell=True)

def main():
	array_id = int(os.environ["SLURM_ARRAY_TASK_ID"])
	print("Array ID: {}".format(array_id))

	bam_file = bam_files[array_id]
	print("Processing {file}".format(file = bam_file))

	index_bam(bam_file)
	filter_bam(bam_file)

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
