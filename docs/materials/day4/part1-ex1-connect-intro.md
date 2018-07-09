---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>


Thursday Exercise 1.1: Getting acquainted with OSG Connect
==========================================================

In this exercise, you'll get acquainted with the OSG Connect submit server (training.osgconnect.net), and the OSG Connect helpdesk.

SSH to `username@training.osgconnect.net` in a command-line terminal.

Look around your 'home' directory
----------

You'll notice the 'public', and 'stash' directories, which may be used in various specific OSG Connect guides. You'll use these spaces for different reasons. **Make sure to read the OSG Connect helpdesk guides** on [data management](https://support.opensciencegrid.org/support/solutions/articles/12000006512-guidelines-for-data-managment-in-osg-storage-and-transfer) and [storage solutions](https://support.opensciencegrid.org/support/solutions/articles/12000002985-storage-solutions-on-osg-home-stash-and-public).

Run `condor_status`
----------

Running `condor_status` by itself on the OSG Connect submit host will give no output, but you can otherwise check with:

``` console
username@training $ condor_status -pool flock.opensciencegrid.org
```

Visit the helpdesk materials
----------------------------

Visit the Help desk page (<https://support.opensciencegrid.org/>) and take a look at the knowledge base articles. You will be using some of these in the next exercises.

Move on to the next exercise when you're ready to submit jobs!

