---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Thursday Exercise 1.4: Try Singularity on the OSG (Optional)
============================================================

In this tutorial, we see how to submit a \[tensorflow\](<https://www.tensorflow.org/>) job on the OSG through \[Singularity containers\](<https://support.opensciencegrid.org/solution/articles/12000024676-singularity-containers>). We currently offer CPU and GPU containers for tensorflow (both based on Ubuntu). Here, we focus on CPU container.

Setup
-----

Make sure you are logged into `training.osgconnect.net` (the OSG Connect submit server for this workshop).

Get the example files and understand the job requirements.
----------------------------------------------------------

Let us utilize the `tutorial` command. In the command prompt, type

``` console
username@training $ tutorial tensorflow-matmul
```

This creates a directory `tutorial-tensorflow-matmul`. Go inside the directory and see what is inside.

``` console
username@training $ cd tutorial-tensorflow-matmul
username@training $ ls -F
```

You will see the following files

``` file
tf_matmul.py            (Python program to multiply two matrices using tensorflow package)
tf_matmul.submit        (HTCondor Job description file)
tf_matmul_wrapper.sh    (Job wrapper shell script that executes the python program)
tf_matmul_gpu.submit    (HTCondor Job description file targeting gpus)
```

NOTE: The file `tf_matmul_gpu.submit` is for gpus, but we will not focus on gpus in this exercise. You are welcome to take a look.

The python script \`tf\_matmul.py\` uses tensorflow to perform the matrix multiplication of a \`2x2\` matrix. Indeed, this is not the best use case of tensorflow. For our purpose of showing how to submit the tensorflow job on the OSG this example is just fine.

We want to run the program on a remote worker machine on the OSG that supports the singularity container. So we set the requirement in our HTCondor description.

``` console
Requirements = HAS_SINGULARITY == True
```

In addition, we also provide the full path of the image via the keyword `+SingularityImage`.

``` console
+SingularityImage = "/cvmfs/singularity.opensciencegrid.org/opensciencegrid/tensorflow:latest"
```

The image is distributed to the remote worker machines through \`cvmfs\`. Although there are multiple ways to obtain the image file for a job on the OSG machine, the image distributed through \`cvmfs\` is preferred.

Submit the tensorflow example job
---------------------------------

Now submit the job to the OSG.

``` console
username@training $ condor_submit tf_matmul.submit 
```

The job will look for a machine on the OSG that has singularity installed. On a matched machine, the job creates the singularity container from the image `/cvmfs/singularity.opensciencegrid.org/opensciencegrid/tensorflow:latest`. Inside this container, the program `tf_matmul.py` begins to execute. 

After your job completed, you will see an output file `tf_matmul.output`. 

``` console
username@training $ cat tf_matmul.output 
result of matrix multiplication
===============================
[[ 1.0000000e+00  0.0000000e+00]
 [-4.7683716e-07  1.0000002e+00]]
===============================

```
The result printed in the output file should be a `2x2` identity matrix.

What container images are available on the OSG?
------
To get an idea on what container images are available on the OSG, take a look at the directory path `/cvmfs/singularity.opensciencegrid.org/opensciencegrid`.  

