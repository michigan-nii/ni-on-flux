# Command summary

## Controlling jobs

| Command | Purpose | Example |
|---------|---------|---------|
| `qsub` | submit a script to be run | `qsub eddy_correct.pbs` |
| `qdel` | delete a queued or running job | `qsub 12345678` |
| `qstat` | show the status of jobs | `qstat $USER` |
| `qstat -t` | shows the array elements separately | `qstat -t $USER` |
| `qstat -n` | shows the node name on which jobs are running | `qstat -n $USER` |
| `cancel-my-jobs` | cancels all the jobs running or queued for the current user | `cancel-my-jobs` |


