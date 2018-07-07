---
status: in progress
---

Tuesday Exercise 1.1: Refresher – Submitting Multiple Jobs
==========================================================

The goal of this exercise is to map the physical locations of some worker nodes in our local cluster.
To do this, you will write a simple submit file that will queue multiple jobs and then manually collate the results.

Where in the world are my jobs?
-------------------------------

To find the physical location of the computers your jobs our running on, you will use a method called *geolocation*.
Geolocation uses a registry to match a computer’s network address to an approximate latitude and longitude.

### Geolocation code

Below is a Python script that geolocates the machine that it is running on, returning
[latitude and longitude coordinates](https://en.wikipedia.org/wiki/Geographic_coordinate_system).
It is not important to understand how the internals of the script works.
When called without any arguments, the script will return the latitude and longitude coordinates of the machine from
which it is run.

```
#!/bin/env python

import sys
import socket
import urllib2
import json
import time

hostnames = set()
if len(sys.argv) == 1:
    hostnames.add(socket.getfqdn())
else:
    for filename in sys.argv[1:]:
        try:
            with open(filename, 'r') as f:
                hostnames.update(set([x.strip() for x in f.readlines()]))
        except IOError:
            pass

hostnames.discard('')
for host in hostnames:
    try:
        ipaddr = socket.gethostbyname(host)
        for i in range(1,4):
            try:
                response = urllib2.urlopen('http://www.freegeoip.net/json/' + ipaddr).read()
                json_response = json.loads(response)
                lat = json_response['latitude']
                lon = json_response['longitude']

                if int(lat) is 0 and int(lon) is 0:
                    exit("www.freegeoip.net could not find associated lat/lon coordinates.")
                print "%s,%s" % (lat, lon)
                break
            except urllib2.HTTPError:
                time.sleep(3**i)
                pass
    except socket.gaierror:
        pass
```

### Geolocating several machines

Now, let’s try to use this Python script and remember some basic HTCondor ideas from yesterday!

1.  Log in to `learn.chtc.wisc.edu`
1.  Create and change into a new folder for this exercise, for example `tuesday-1.1`
1.  Save the Python script above as a file named `location.py`
1.  As always, ensure that your script has the proper permissions (hint: try running it from the command line)
1.  Create a submit file that generates **ten** jobs that run `location.py` and uses the `$(Process)` macro to write
    different `output` and `error` files.
    Try to do this step without looking at materials from yesterday.
    But if you are stuck, see [yesterday’s exercise 2.4](/materials/day1/part2-ex2-queue-n.md).
1.  Submit your jobs and wait for the results

### Collating your results

Now that you have your results, it's time to summarize them.
Rather than inspecting each output file individually, you can use the `cat` command to print the results from all of
your output files at once.
If all of your output files have the format `location-#.out` (e.g., `location-10.out`), your command will look something
like this:

``` console
user@learn $ cat location-*.out
```

The `*` is a wildcard so the above cat command runs on all files that start with `location-` and end in `.out`.
Additionally, you can use `cat` in tandem with the `sort` and `uniq` commands to print only the unique results:

``` console
user@learn $ cat location-*.out | sort | uniq
```

Mapping your results
--------------------

To visualize the locations of the machines that your jobs ran on, you will be using <http://www.mapcustomizer.com/>.
Copy and paste the collated results into the text box that pops up when clicking on the 'Bulk Entry' button on the
right-hand side.
Are the results what you expected?

Next exercise
-------------

Once completed, move onto the next exercise: [Logging in to the OSG Submit Machine](/materials/day2/part1-ex2-login-scp.md)

