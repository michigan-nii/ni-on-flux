# Example 2

## Setting up your list

This example will illustrate how to use a job array to run the same
command or set of commands on a list of subjects from a file. This
is possibly the most common scenario for using the cluster.

The purpose of this is to illustrate how to use the list of subjects,
so expect that there will be much more complicated tasks done to
each than what is shown here.

This scenario requires a list of something. Often the list will be
of subjects, but it may also be a list of files, which is what we
will use here.

Change to the `ni-for-flux/example02` folder.

For this example, you will want to have a list of the files for the
covert verb generation task.  They are named,

```
sub-01_ses-retest_task-covertverbgeneration_bold.nii.gz
sub-01_ses-test_task-covertverbgeneration_bold.nii.gz
sub-02_ses-retest_task-covertverbgeneration_bold.nii.gz
. . . .
```

They are in directories of their own, too, because this is the new, cool
BIDS format, so you will find the file for the SES retest task for
covert verb generation for subject 02 under

```
ds000114_R2.0.1/sub-02/ses-retest/func
```

in this case, the Linux command `find` is our friend, and we use it to
get a list of all the files that end with `covertverbgeneration_bold.nii.gz`

```
$ find ds000114_R2.0.1/sub-?? -name \*covertverbgeneration_bold.nii.gz \
    | sort > covert_verb.lst
```

## Job arrays

When you have something that needs to be done on a large number of listable
names -- subject IDs, task file names, etc -- you can use a job array.  A
job array is used to generate some number of jobs, all of which are the same
except for the numerical value of their index, which can be either a list of
discrete values, e.g., 1, 5, 11, or a range of sequential values, e.g., 1-5.

When we have a list of files or subjects in a file, we can use that and the
array index to make one PBS script work for many values.  An example will help.

Suppose we have a file of subject IDS, 001 through 005. The file itself will
not have line numbers, but if we could find a way to extract from that file
the <em>N</em>th line, say line number three, then would could use that in
our script to process subjects based on the value of the job array.

```
Line no     Line contents   First N     Last of the first N
-----------------------------------------------------------
[1]         sub001          sub001
[2]         sub002          sub002 
[3]         sub003          sub003           sub001
[4]         sub004
[5]         sub005
```

We can do this by using the `head` command to take the first N lines, say
three, then if we take just the last one of those, we have what we want.
You can see what we get in the table above; here is what it would look
like with our real list of files.

```
$ head -3 covert_verb.lst | tail -1
ds000114_R2.0.1/sub-02/ses-retest/func/sub-02_ses-retest_task-covertverbgeneration_bold.nii.gz
```

How is that useful here? If you set your array to have 5 elements, numbered
1, 2,...,5, then each job will create an environment variable called
`$PBS_ARRAYID` that will have that number in it.

We are outside a job, so we can test this by setting the value of
`PBS_ARRAYID` ourselves and testing it.

```
$ PBS_ARRAYID=3
head -${PBS_ARRAYID} covert_verb.lst | tail -1
sub001
```

to get the <em>N</em>th filename out of covert_verb.lst, and we can put that into a
variable called current_file this way,

```
current_file=$(head -${PBS_ARRAYID} covert_verb.lst | tail -1)
```

and we can now use that with our analysis command, which for illustration
just prints the header information.

```
fslinfo $current_file
```

When you are done testing, make sure that you **unset** the `PBS_ARRAYID`
variable.

```
$ unset PBS_ARRAYID
```

so there is no chance it will carry over into your job from your test!

So, put all this into a job script, you will want to copy the template
file and add the commands we need to it.  We have already generated the
list of files, so that is not needed, but we need to add a line to tell
PBS the range of array numbers to use, and we need to set the value of
`current_file` for each job, and finally we need to use that value in
our analysis.

```
$ cp ../template.pbs array_test.pbs
```

We will use `test_array` as the job name.

The array is specified with

```
#PBS -t 1-5
```

I add that right after the `-l` lines.  Then we want to add the
command to get the <em>N</em>th filename from the list, and the
commands we wish to run, which we did as

```
##  Get the Nth element from the file list
echo "Getting current file with: head -${PBS_ARRAYID} covert_verb.lst | tail -1"
current_file=$(head -${PBS_ARRAYID} covert_verb.lst | tail -1)

##  Run fslinfo on that file
echo Running:  fslinfo $current_file
fslinfo $current_file
```

## Running your job

You can now `qsub` that file, which will generate a new job ID that looks like
```
$ qsub array_test.pbs 
32319640[].nyx.arc-ts.umich.edu
```

The brackets indicate that it is an array.  Each element is its own, separate
job, and to see those, you use

```
$ qstat -t 32319640[]
Job ID                    Name             User            Time Use S Queue
------------------------- ---------------- --------------- -------- - -----
32319640[1].nyx.arc-ts.umich. test_array-1     grundoon               0 Q flux           
32319640[2].nyx.arc-ts.umich. test_array-2     grundoon               0 Q flux           
32319640[3].nyx.arc-ts.umich. test_array-3     grundoon               0 Q flux           
32319640[4].nyx.arc-ts.umich. test_array-4     grundoon               0 Q flux           
32319640[5].nyx.arc-ts.umich. test_array-5     grundoon               0 Q flux           
```

You would then be able to use

```
$ qstat 32319640[2]
```

to see the status of the second array job.  The output files from this would
be

```
test_array.o32319640-1  test_array.o32319640-3  test_array.o32319640-5
test_array.o32319640-2  test_array.o32319640-4
```

Did you get an error in them? Something like,

```
/var/spool/torque/mom_priv/jobs/32319805-1.nyx.arc-ts.umich.edu.SC:
  line 95: fslinfo: command not found
```

The first part of that scary message is just the name of your
script after PBS copied it to the compute node to run.  The part you
should read is

```
  line 95: fslinfo: command not found
```

Ah, ha!  Remember to check which modules you need to have loaded before
submitting your job; put that into a comment in your PBS file so you
can use it from there.

It is not a bad idea to clear your module list and use only the modules
you will really need.  Don't forget to put the module list into the comment
in your PBS file.

```
$ module purge
$ module load fsl/5.0.11
$ module list
$ qsub array_test.pbs
```

That should produce better results.

What do you think?  Would it be a better reminder to put the comment
about which modules to load at the _top_ of the PBS script rather than
down where the software gets run?  Would that make you remember to
check that better than its current location?
