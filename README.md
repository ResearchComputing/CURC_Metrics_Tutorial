
## Tutorial: Getting the most out of CU Research Computing resources

### Objectives: 

_To better understand how to access information on:_

* your high-performance computing resource consumption
* average wait times in the CURC queues and your relative "priority"
* the efficiency of your research workflows

### Methods:

_We will explore two groupings of tools:_

1. __Web-based__: https://xdmod.rc.colorado.edu

  * How long are queue waits right now?
  * How many Service Units ("SUs") have I used over the past month? 

2. __Command-line__: the "slurmtools" module

  * How many SUs have I used recently? 
  * Who is using all of the SUs on my group's account?
  * What jobs have I run over the past few days?
  * What is my priority?  
  * How efficient are my jobs? 

### Tutorials:

#### Web-based tools: https://xdmod.rc.colorado.edu

XXXX

#### Command line tools: the `slurmtools` module

__Step 1__: If you have a CURC account, login as you normally would using your identikey and Duo from a terminal: 

```bash
ssh ralphie@login.rc.colorado.edu
```
or, if you _do not_ have a CURC account, ask the instructor for a temporary username and password and login as follows:

```bash
ssh user00<NN>@tlogin1.rc.colorado.edu
```

__Step 2__: Load the slurm module for either Summit or Blanca (whichever HPC resource you want to query metrics about):

```bash
module load slurm/summit
```
or
```bash
module load slurm/blanca
```

__Step 3__: Load the `slurmtools` module:

```bash
module load slurmtools
```

...you'll see the following informational message:

```
You have sucessfully loaded slurmtools, a collection of functions
 to assess recent usage statistics. Available commands include:

 'suacct' (SU usage for each user of a specified account over N days)

 'suuser' (SU usage for a specfied user over N days)

 'seff' (CPU and RAM efficiency for a specified job)

 'jobstats' (job statistics for all jobs run by a specified user over N days)

 'levelfs' (current fair share priority for a specified user)


 Type any command without arguments for usage instructions
 ```

__Step 4__: Let's get some metrics!

___How many Service Units (core hours) have I used?___

Type the command name for usage hint:
```bash
suuser
```
```
Purpose: This function computes the number of Service Units (SUs)
consumed by a specified user over N days.

Usage: suuser [userid] [days, default 30]
Hint: suuser ralphie 15
```

Check my usage for the last 365 days:
```bash
suuser monaghaa 365
```
```
SU used by user monaghaa in the last 365 days:
Cluster|Account|Login|Proper Name|Used|Energy|
summit|admin|monaghaa|Andrew Monaghan|15987|0|
summit|ucb-testing|monaghaa|Andrew Monaghan|3812|0|
summit|tutorial1|monaghaa|Andrew Monaghan|3812|0|
summit|ucb-general|monaghaa|Andrew Monaghan|5403|0|
```

This output tells me that:
* I've used "SUs" across four different accounts over the past year
* My usage by account varied from 3,812 SUs to 15,987 SUs

___Who is using all of the SUs on my groups' account?___

Type the command name for usage hint:
```bash
suacct
```
```
Purpose: This function computes the number of Service Units (SUs)
consumed by each user of a specified account over N days.

Usage: suacct [account_name] [days, default 30]
Hint: suacct ucb-general 15
```

Check `admin` account usage over past 180 days:
```bash
suacct admin 180
```
```
SU used by account (allocation) admin in the last 180 days:
Cluster|Account|Login|Proper Name|Used|Energy
summit|admin|||763240|0
summit| admin|coke4948|Corey Keasling|84216|0
summit| admin|frahm|Joel Frahm|24|0
summit| admin|holtat|Aaron Holt|9832|0
summit| admin|joan5896|Jonathon Anderson|9357|0
summit| admin|monaghaa|Andrew Monaghan|9357|0
```

This output tells me that:
* Five users used the account in the past 180 days.
* Their usage ranged from 24 SUs to 84,216 SUs


___What jobs have I run over the past few days?___

Type the command name for usage hint:
```bash
jobstats
```
```
Purpose: This function shows statistics for each job
run by a specified user over N days.

Usage: jobstats [userid] [days, default 5]
Hint: jobstats ralphie 15
```

Check my jobstats for the past 35 days:
```bash
jobstats monaghaa 35
```
```
job stats for user monaghaa over past 35 days
jobid        jobname  partition    qos          account      cpus state    start-date-time     elapsed    wait
-------------------------------------------------------------------------------------------------------------------
8483382      sys/dash shas         normal       ucb-gener+   1    TIMEOUT  2021-09-14T09:32:09 01:00:16   0 hrs
8487254      test.sh  shas         normal       ucb-gener+   1    COMPLETE 2021-09-14T13:21:12 00:00:02   0 hrs
8487256      spawner- shas-inte+   interacti+   ucb-gener+   1    TIMEOUT  2021-09-14T13:22:11 12:00:22   0 hrs
8508557      test     shas-test+   testing      ucb-gener+   2    COMPLETE 2021-09-16T10:41:45 00:00:00   0 hrs
8508561      test     shas-test+   testing      ucb-gener+   24   CANCELLE 2021-09-22T10:07:03 00:00:00   143 hrs
8508569      test     shas         normal       ucb-gener+   4096 FAILED   2021-09-16T10:42:46 00:00:00   0 hrs
8508575      test     shas         normal       ucb-gener+   8192 FAILED   2021-09-16T10:43:17 00:00:00   0 hrs
8508593      test     shas         normal       ucb-gener+   4096 CANCELLE 2021-09-16T10:44:47 00:00:00   0 hrs
8508604      test     shas         normal       ucb-gener+   2048 CANCELLE 2021-09-16T10:45:40 00:00:00   0 hrs
8512083      spawner- shas-inte+   interacti+   ucb-gener+   1    TIMEOUT  2021-09-16T16:55:37 04:00:23   0 hrs
8579077      sinterac shas-test+   testing      ucb-gener+   1    COMPLETE 2021-09-24T15:26:32 00:00:47   0 hrs
8627076      sinterac sgpu-test+   testing      ucb-gener+   24   CANCELLE 2021-10-04T12:17:30 00:10:03   0 hrs
8672525      spawner- shas-inte+   interacti+   ucb-gener+   1    CANCELLE 2021-10-08T13:29:13 00:07:25   0 hrs
8800741      spawner- shas-inte+   interacti+   ucb-gener+   1    CANCELLE 2021-10-19T08:11:44 01:48:38   0 hrs
```

This output tells me that:
* I've run 14 jobs in the past 35 days
* Most jobs had queue waits of < 1 hour
* The number of cores requested ranged from 1-->8192
* The elapsed times ranged from 0 hours to 1 hour and 48 minutes


___What is my priority?___

Type the command name for usage hint:
```bash
levelfs
```
```
Purpose: This function shows the current fair share priority of a specified user.
A value of 1 indicates average priority compared to other users in an account.
A value of < 1 indicates lower than average priority
	(longer than average queue waits)
A value of > 1 indicates higher than average priority
	(shorter than average queue waits)

Usage: levelfs [userid]
Hint: levelfs ralphie
```

Check my priority:
```bash
levelfs monaghaa
```
```
monaghaa
admin LevelFS: inf
ucb-general LevelFS: 44.796111
tutorial1 LevelFS: inf
ucb-testing LevelFS: inf
```

This output tells me:
* I haven't used `admin`, `tutorial1`, or `ucb-testing` for more than a month, and therefore I have very high ("infinite") priority. 
* I have used `ucb-general` but not much. My priority is >> `, therefore I can expect lower-than-average queue waits compare to average ucb-general waits.

___How efficient are my jobs?___

Type the command name for usage hint:
```bash
seff
```
```
Usage: seff [Options] <Jobid>
       Options:
       -h    Help menu
       -v    Version
       -d    Debug mode: display raw Slurm data
```

Now check the efficiency of job 8636572:
```bash
seff 8636572
```
```
Job ID: 8636572
Cluster: summit
User/Group: ralphie/ralphiegrp
State: COMPLETED (exit code 0)
Nodes: 1
Cores per node: 24
CPU Utilized: 04:04:05
CPU Efficiency: 92.18% of 04:24:48 core-walltime
Job Wall-clock time: 00:11:02
Memory Utilized: 163.49 MB
Memory Efficiency: 0.14% of 113.62 GB
```

This output tells me that:
* the 24 cores reserved for this job were 92% utilized (anything > 80% is pretty good)
* 163.49 MB RAM was used of 113.62 GB RAM reserved (0.14%). This job is "cpu bound" so the memory inefficiency is not a major issue.

### Final items:

* Questions?

* Please consider providing feedback on this tutorial via this brief survey: http://tinyurl.com/curc-survey18

* Links
  * [CURC XdMOD documentation](https://curc.readthedocs.io/en/latest/gateways/xdmod.html)
  * [XdMOD user manual](https://xdmod.rc.colorado.edu/user_manual/index.php) 
