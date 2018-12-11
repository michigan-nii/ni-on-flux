# Simple example

We will start with a simple example.  You decide you want to use MRtrix,
and one of the first things you will need to do is to correct or field
bias.

Consulting the wizards down the hall, you find out that you need to run
the command

```
$ dwibiascorrect -ants dwi.mif dwi_biascorrected_fba.mif
```

This is just an example to illustrate how to find things.  Change to
the `ni-for-flux/example01` directory.  The `dwi.mif` file exists,
so all we need to do is figure out how to run the software.

The first thing you need to do is to locate the MRtrix software on
Flux.  Neuroimaging software will be provided via a _module_, which
sets up your environment to run whichever software is in the module.

To find out whether software is installed on Flux, you should use
something like

```
$ module keyword neuroimaging
```

The output from the will show you the names of the software available
and which versions are available.  The entry for MRtrix is

```
  mrtrix: mrtrix/3.0_RC2, mrtrix/3.0_RC3
    MRtrix provides a set of tools to perform various advanced diffusion MRI analyses.
```

The module name is on the first line, to the left of the colon, and the versions
are listed to the right of the colon, where the format is
`<module name>/<module version>`.  So, the latest version is `mrtrix/3.0_RC3`.

You can see the contents of the module's help with

```
$ module spider mrtrix/3.0_RC3
```

and see that it can be loaded directly.  Loading a module will make the software
available.

```
$ module load mrtrix/3.0_RC3
Lmod has detected the following error:  Cannot load module "mrtrix/3.0_RC3"
 without these module(s) loaded:
   gcc/5.4.0 fftw/3.3.4/gcc/5.4.0
. . . .
```

So, we got an error.  That tells us that we need to load some other modules
first.  They are listed on the third line of output, and you can copy and
past them, as in

```
$ module load gcc/5.4.0 fftw/3.3.4/gcc/5.4.0
```

then retry loading `mrtrix/3.0_RC3`.  You can see what modules are loaded
with

```
$ module list

Currently Loaded Modules:
  1) gcc/5.4.0   2) fftw/3.3.4/gcc/5.4.0   3) mrtrix/3.0_RC3
```

When you can, you should test commands on one subject, or using a smaller
data set, or fewer iterations, from the command line.  We will do that
here with

```
$ dwibiascorrect -ants dwi.mif dwi_biascorrected_fba.mif
dwibiascorrect: 
dwibiascorrect: Note that this script makes use of commands / algorithms
 that have relevant articles for citation; INCLUDING FROM EXTERNAL SOFTWARE
 PACKAGES. Please consult the help page (-help option) for more information.
dwibiascorrect: 
dwibiascorrect: [ERROR] Could not find ANTS program N4BiasFieldCorrection;
 please check installation
```

What's up with that?

Looks like you need to find ANTS.  Use `module keyword` to verify that it is
available, then load your desired version.

```
$ module keyword ants
$ module load ANTs/2.1.0
```

Now retry `dwibiascorrect`, which should now run.  Note that for programs
that take a long time, a lot of memory, many processors, or a GPU, you
have to submit a job to test.

To create a submittable job from this, copy the template PBS script
you created into the example directory and call it `bias_correction.pbs`.

```
$ cp ../template.pbs ./bias_correction.pbs
```

Now you need to edit `bias_correction.pbs` to add the `dsibiascorrect`
command that you just tested after the line that reads

```
####  Commands your job should run follow this line
```

That's the least you need to do.  You can submit your job now.  Oh,
wait!  Many programs, FSL and MRtrix among them, won't run if you
have already run them, so you should remove the `dwi_biascorrected_fba.mif`
file created by the running `dwibiascorrect` manually before you
submit your job.

It is generally good to change the job name, add a comment that lists
the modules needed, and comment what your commands are doing.  To see
what is different from the minimum, you can compare your `bias_correction.pbs`
to the one we would use with

```
$ diff bias_correction.pbs solution/bias_correction.pbs
```
