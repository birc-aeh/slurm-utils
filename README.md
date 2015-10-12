# slurm-utils
A few utilities for use on (hopefully) any SLURM cluster.

The only requirements are python 2.6+ and that the standard slurm commands
(`squeue`,`sinfo`,...) are available through PATH.
If the `hostlist` module is available to python gnodes will use it to expand
list of nodes, otherwise it will use an included function with some
limitations.

# gnodes
The `gnodes` script gives a visual representation of your cluster.
It shows you available memory and allocated/loaded cores for every every node
in each partition.
The layout is adjusted to the terminal width, but might get very tall if you
have a lot of nodes of course.

It optionally also takes multiple search parameters that can be usernames, job
ids or node names. Nodes that are running either the mentioned job or any job
from the user will be highlighted.

Unallocated cores are marked with `.`, allocated cores with no load
are marekd with `_`, loaded cores are marked with `O` and if the load
goes above 1.5 the allocated number of cores it is marked with `!`.

# jobinfo
The `jobinfo` script tries to collect information for a full job combining
information from the SLURM accounting system and the live stats from sstat.

Example output:

    [aeh@fe1 ~]$ jobinfo 11983512
    Name                : bash
    User                : aeh
    Partition           : normal
    Nodes               : s02n[45-48,51-53]
    Cores               : 50
    State               : FAILED
    Submit              : 2015-10-12T21:21:18
    Start               : 2015-10-12T21:21:23
    End                 : 2015-10-12T21:24:14
    Reserved walltime   : 2-00:00:00
    Used walltime       :   00:02:51
    Used CPU time       :   00:00:59
    % User (Computation): 83.22%
    % System (I/O)      : 16.78%
    Mem reserved        : 100M/node
    Max Mem used        : 25.18M (s02n45,s02n47,s02n48,s02n51,s02n53)
    Max Disk Write      : 16.00M (s02n45)
    Max Disk Read       : 2.00M (s02n45)

It has mostly been tested on batch jobs without any sub-steps so please send
feedback.
