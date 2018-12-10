# Command summary

## Controlling jobs

| Command | Purpose | Example |
|:---------|---------|---------|
| `qsub` | submit a script to be run | `qsub eddy_correct.pbs` |
| `qdel` | delete a queued or running job | `qsub 12345678` |
| `qstat` | show the status of jobs | `qstat $USER` |
| `qstat -t` | shows the array elements separately | `qstat -t $USER` |
| `qstat -n` | shows the node name on which jobs are running | `qstat -n $USER` |
| `cancel-my-jobs` | cancels all the jobs running or queued for the current user | `cancel-my-jobs` |

## Controlling software availability

These are subcommands to the `module` command.

| Subcommand | Purpose | Example |
|:---------|---------|---------|
| `keyword` | search for term in all modules | `module keyword fmri` |
| `spider` | more detail about a specific package | `module spider fsl` |
| `available` | modules that are available to load | ` module available ANTs` |
| `load` | load an available module | `module load freesurfer` |
| `list` | list all loaded modules | `module list` |
| `purge` | unload all loaded modules | `module purge` |
| `save [collection_name]` | save a named collection of modules | `module save surf` |
| `savelist` | list the names of saved collections<br> (carefull! if you stop at `save`<br> it will do something you don't want| `module savelist` |
| `help [module_name]` | print help about the `module` command or the named module | `module help mrtrix` | 
