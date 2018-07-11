---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Thursday Exercise 1.2:  Run a simple quickstart job on OSG Connect
-----

For this exercise, you can also follow the online guide from the OSG Connect helpdesk that will acquaint you with submission on the OSG Connect submit server. Please use the submit host training.osgconnect.net instead of login.osgconnect.net for the workshop.

Setup
-----

SSH into `training.osgconnect.net` (the OSG Connect submit server for this workshop).


Get the files for quickstart example
-----

We will get the example files using the `tutorial` command.

``` console
username@training $ tutorial quickstart
```

This creates a directory `tutorial-quickstart`. Go inside the directory `tutorial-quickstart` and see what is inside.

``` console
username@training $ cd tutorial-quickstart
username@training $ ls -F 
```

You will see the following contents:

``` file
Images/  osg-template-job.submit  short.sh  tutorial02.submit log/     README.md   tutorial01.submit  tutorial03.submit
```

We will focus our attention on `short.sh` (execution file) and `tutorial01.submit`. We will not worry about other files in this exercise. Feel free to take a look at other files if you are interested.

Job Execution File
------------------

Take a look at the job execution file `short.sh`.

``` console
username@training $ cat short.sh 
```

This is a shell script, quite ordinary. Run this shell script locally to see what it does.

``` console
username@training $ chmod +x short.sh 
username@training $ ./short.sh 
```

Submitting the job on the OSG
-----------------------------

The job description file `tutorial01.submit` executes the shell script `short.sh` as a vanilla universe job. Take a look at the job description file.

``` console
username@training $ cat tutorial01.submit 
```

Now run this job on the OSG.

``` console
username@training $ condor_submit tutorial01.submit 
```

Once your job has finished, you can look at the files that HTCondor has returned to the working directory. If everything was successful, it should have returned:

``` file
job.output (An output file for each job's output)
job.error: (An error file for each job's errors)
job.log: (A log file for each job's log)
```

Read the output file.

``` console
username@training $ cat job.output 
```

Observe the difference between the outputs from running the job on the OSG and running locally. (Hint: Check the username, id, work directory etc.)

