# slurm-utils
A few utilities for use on (hopefully) any SLURM cluster.

The only requirements are python 2.6+ and that the standard slurm commands
(`squeue`,`sinfo`,...) are available through PATH.
If the `hostlist` module is available to python `gnodes` will use it to expand
lists of nodes, otherwise it will use an included function with some
limitations.

# installation
The two scripts are self-contained, so installation is done by downloading the
latest version and then marking the two files as executable.

    wget https://raw.githubusercontent.com/birc-aeh/slurm-utils/master/gnodes
    wget https://raw.githubusercontent.com/birc-aeh/slurm-utils/master/jobinfo
    chmod +x gnodes jobinfo
    mv gnodes jobinfo ~/.local/bin/ # or another folder in your $PATH

Additionally you might have to modify the shebang line of the two files. They
assume the presence of a python3 in the path but RHEL8 for example doesn't
guarantee any python executable other than `/usr/libexec/platform-python`.

# gnodes
The `gnodes` script gives a visual representation of your cluster.
It shows you available memory and allocated/loaded cores for every every node
in each partition.
The layout is adjusted to the terminal width, but might get very tall if you
have a lot of nodes of course.

It optionally also takes multiple search parameters that can be usernames, job
ids or node names. Nodes that are running either the mentioned job or any job
from the user will be highlighted.

Example output:

    +- normal - 16 cores & 23GB ------+---------------------------------+
    | norm_54   23G  ........_____OOO | norm_70   23G  ................ |
    | norm_55   23G  ........_____OOO | norm_71   23G  ................ |
    | norm_56   23G  ........_____OOO | norm_72   23G  ................ |
    | norm_57   23G  ............___O | norm_73   23G  ................ |
    | norm_58   23G  ........____OOOO | norm_74   23G  ................ |
    | norm_59   23G  ........____OOOO | norm_75   23G  ................ |
    | norm_60    0G  _______________O | norm_76   23G  ................ |
    | norm_61   23G  ........____OOOO | norm_77   23G  ................ |
    | norm_62   23G  ......_____OOOOO | norm_78   23G  ................ |
    | norm_63   23G  ................ | norm_79   23G  ................ |
    | norm_64   23G  ................ | norm_80   23G  ........!!!!!!!! |
    | norm_65   23G  ................ | norm_81   23G  ........!!!!!!!! |
    | norm_66   23G  ................ | norm_82   23G  ................ |
    | norm_67   23G  ................ | norm_83   23G  ........____OOOO |
    | norm_68   23G  ................ | norm_84   23G  ........____OOOO |
    | norm_69   23G  ................ | norm_85   23G  ........____OOOO |
    +---------------------------------+---------------------------------+

    +- fancyfancy - 40 cores & 62GB --------------------------+
    | fancy_0    0G  ____________________________OOOOOOOOOOOO |
    | fancy_1    0G  ___________________________OOOOOOOOOOOOO |
    | fancy_2    0G  ____________________________OOOOOOOOOOOO |
    | fancy_3    0G  __________________________OOOOOOOOOOOOOO |
    | fancy_4    0G  ____________________________OOOOOOOOOOOO |
    | fancy_5    0G  ___________________________OOOOOOOOOOOOO |
    +---------------------------------------------------------+


Unallocated cores are marked with `.`, allocated cores with no load
are marked with `_`, loaded cores are marked with `O` and if the load
goes above 1.5 times the allocated number of cores it is marked with `!`.

# jobinfo
The `jobinfo` script tries to collect information for a full job combining
information from the SLURM accounting system and live stats from `sstat` if the
job is still running.

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
