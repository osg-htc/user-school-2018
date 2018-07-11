---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Thursday Exercise 1.3: Try an OSG Connect Software Module
=========================================================


In this exercise, you'll use a software package called GROMACS that was already installed on OASIS. GROMACS is a molecular dynamics simulation program.

Setup
-----

Make sure you are logged into `training.osgconnect.net` (the OSG Connect submit server for this workshop).

Practice loading the GROMACS module on OSG Connect submit host
--------------------------------------------------------------

Before proceeding, make sure to read over the OSG Connect help desk guide on [Accessing Software using Distributed Environment Modules](https://support.opensciencegrid.org/support/solutions/articles/5000634394-accessing-software-using-distributed-environment-modules).

1. Run the `module avail` command to see available modules and determine which `gromacs` version will be the default (designated by a `(D)`).

2. Before loading the `gromacs` module, check your "PATH" variable with:

``` console
username@training $ echo $PATH
```

3. Now load the `gromacs` module, which will give you the default version, and check your list of modules:

``` console
username@training $ module load gromacs
username@training $ module list
```

4. Recheck your "PATH" variable. What has changed? Is the default version of `gromacs` visible?

5. When you're done, unload the module and check that you no longer have the module loaded:

``` console
username@training $ module unload gromacs
username@training $ module list
username@training $ echo $PATH
```

Submit a job that uses GROMACS from OASIS
-----------------------------------------

Now let us see how to do a job submission on the OSG that uses GROMACS on OASIS (you don't have to transfer GROMACS source or the binary along with the job).

We will get the example files using the `tutorial` command.

``` console
username@training $ tutorial gromacs
```

This creates a directory `tutorial-gromacs`. Go inside the directory and see what is inside.

``` console
username@training $ cd tutorial-gromacs
username@training $ ls -F 
```

You will see the following files:

``` file
1cta_nvt.tpr  Figs/  gromacs_job.sh  gromacs_job.submit  README.md
```

Take a look at the job description file

``` console
username@training $ cat gromacs_job.submit 
```

The modules are loaded in the wrapper script before executing the actual job. In this example, gromacs is loaded before running the actual simulation.

``` console
username@training $ cat gromacs_job.sh
```

The following requirement is added to find a machine that has the modules:

``` console
requirements = HAS_MODULES == True
```

Check the job description file.

``` console
username@training $ cat gromacs_job.submit 
```

Let's submit the job.

``` console
username@training $ condor_submit gromacs_job.submit 
```

After the job completed, you will see the output files (including gro, cpt and trr files) from GROMACS in your work directory.

