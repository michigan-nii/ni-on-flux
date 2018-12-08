
# What is a cluster anyway?

![What are clouds made of?](./images/cloud.jpg)

## Cluster components

Clusters and clouds are very similar. The cloud is _mostly_ Linux,
whereas the Flux cluster is _all_ Linux servers.

The following diagram shows a generic cluster configuration.

![Generic cluster configuration](./images/hpc_system_diagram.png)

We call specific physical machines _nodes_, and there are three types of
node on Flux of which you should be aware:  login, data transfer, and
compute.

As you can see from the diagram of the cluster, your point of contact is a
_login node_, on which you enter regular commands, as you would at your
own computer.  The login node is where you edit files, make directories,
and get your work organized.  The login nodes require you to have Duo
Two-Factor Authentication to log in.

Login nodes have policies regarding their use. On Flux the policy says these
are the intended uses:  edit files, compile source code, and run test
programs on small data sets for short periods of time to uncover syntax
errors and the like. A login node is not where you should run something
like `recon-all`, which can take a day, but it is OK to run something
small, like `bet`, which will finish in a minute or two.

For most work on the cluster, you will create a script, variously called
a _batch_ script or a _PBS_ script, which you will _submit_ to the cluster
to be run on a compute node or set of compute nodes.  You won't normally
log into a compute node.

These are the typical steps.

1. Copy data to Flux

1. Log onto a login node

1. Create the files needed to run your analyses

1. Submit your PBS script to be run

1. Examine results as needed and appropriate.

1. Copy result files, output, and log files you need to keep from
   Flux.

1. Delete files from any temporary locations.

## Disk storage

There is network storage space that is shared among the login and compute
nodes, so files you see from a login node are also visible from compute
nodes.

1. Your _home directory_ will be `/home/username`.  There is an 80 GB quota
   on your home directory.  Unless you have only a smallish amount of data
   you probably don't want to work from your home directory.

1. On Flux you will have an _account_ that gets charged for your use. For
   each account you can use, there will be a _scratch directory_.  People
   in LSA can use and account paid for by LSA, and you will have a
   scratch directory for it at `/scratch/lsa_flux/username`.

1. You may also have access to disk space paid for by someone.  That space
   is typically under `/nfs`, and it may be shared with computers in your
   lab so you would not need to copy files to/from Flux.

