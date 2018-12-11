# PBS script template

## PBS options

One good way to work with batch processing is to create template files
for the PBS script.  Those will contain the job specifications that will
be fairly constant, so you can concentrate on the imaging analysis part.

You should copy the workshop files to your home directory with the
following command (the `$` indicates the prompt; do not copy it).

```
$ cp -r /nfs/turbo/arcts-dads-nii-open/ni-for-flux ~
```

then change to the `nii-for-flux` directory that creates

```
$ cd ni-for-flux
```

The file `template.pbs` can be used as a starting point for your own
PBS scripts. Lines that begin with a `#` are comments, as in any shell
script.  Blank lines are ignored. The job manager looks for the special
string `#PBS` at the beginning of lines at the beginning of the file,
and it reads those as instructions about setting up your job. The
non-comments from the file are repeated here.

```
#!/bin/bash
#PBS -N dwi2mask
#PBS -l nodes=1:ppn=1
#PBS -l mem=4gb
#PBS -l walltime=15:00
#PBS -A training_flux
#PBS -q flux
#PBS -m ea
#PBS -j oe
#PBS -V
```

Those are all the instructions for how to run you job and what it should
run on.  We'll call the text to the right of the `#PBS` the _option_, so
the first option is `-N` for job name, and in this case the value for the
option is `dwi2mask`. Names should be less than 20 characters, they should
contain only letters, numerals, or the underscore. The name will be used
as part of the output filename.

There are three `-l` option lines. These are used to specify how much of
which computers you want your job to have and for how long.  The first
specifies one physical node (`nodes=1`) and one processor per node (`ppn=1`).

The second specifies the total memory for the job, in this case 4 GB.
When requesting any size job with `nodes=1`, you should specify memory
with `mem=`. Most NI software will only be able to use one node.

The third `-l` line says how long the system should give your job to
complete. If it does not complete in that time, your job is killed.
Time is specified as `dd:hh:mm:ss`. The example shows 15 minutes; 
`walltime=1:15:00` would specify 1 hour and 15 minutes.

To run jobs, someone must pay for time, and that is done with an
account -- not to be confused with your login or user account.  That
is specified with `-A`. When you get access to an account, the person
giving you access should provide the name.  It will end with `_flux`
possibly followed by some other letter(s).  For workshops, we
provide `training_flux`.  Many of you will use `lsa_flux`.

The `-q` option says which group of computers to get in line to use.
This value should be whatever follows the underscore in the account
name; for the two examples above, that will be `flux`. If you were
going to run something with GPUs, then you might use

```
#PBS -A lsa_fluxg
#PBS -q fluxg
```

The `-m` options specifies when you will get e-mail about your job.
We recommend that you use `ea`, as shown, to get mail if your job
ends with an error and when it ends normally.  There is useful
information in the mail sent when a job ends.

The `-j oe` option is used to put both regular output and error
messages into the same file.  Just Do It.

Finally, the `-V` options insures that all the software that is
available when you submit your job is also available to your job
when it runs.

## Script options

Those are all of the things that govern how your gets run.  There
are also a few things in the file that are part of your job.  These
are not required, but they are useful.

There are two conditional blocks that will run only when your PBS
script is run within a job.  The first is

```
if [ -e "$PBS_NODEFILE" ] ; then
    echo "Running on"
    uniq -c $PBS_NODEFILE
fi
```
and it simply prints the name of the machine(s) on which your job
ran and how many processors on each.

When a job starts, it will start in your home directory.  If your
data is elsewhere, you either need to specify full paths to it or
change directory to where your data is. It is a good practice to
change to the directory where you want to start, and use the second
block,

```
if [ -d "$PBS_O_WORKDIR" ] ; then
    cd $PBS_O_WORKDIR
fi
```

to change there; the special variable `PBS_O_WORKDIR` contains
the name of the directory you were in when you submitted your
job.  That's very convenient, as you only have to worry about
being in the right directory when you submit and not about having
to change the directory name in your scripts.

There are two other commands we urge you to put in all your
PBS scripts, and those are

```
module list
echo "Running from $(pwd)"
```

The first lists the software you have available from inside your
job, and the second will confirm you are working from where you
think you should.

## Testing your setup

It is always a good idea to test your setup.  The template file
that we just created will generate some output, so you should submit
it now using

```
$ qsub template.pbs
32317825.nyx.arc-ts.umich.edu
```

The output from `qsub` is the job ID, and the number at the beginning
of it, 32317825, in the example above will be used with other
`q`-commands/

It will take up to 5 minutes for your job to start.  You can check on
status with

```
$ qstat 32317825
```

and if you decide that there was something wrong, you can delete it with

```
$ qdel 32317825
```

To see the status of all of your jobs, you should use

```
$ qstat -u $USER
```

However, let us move on how to find and use software on Flux.
