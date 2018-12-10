## Controlling software availability

These are subcommands to the `module` command.

| Subcommand | Purpose | Example |
|---------|---------|---------|
| `keyword` | search for term in all modules | `module keyword fmri` |
| `spider` | more detail about a specific package | `module spider fsl` |
| `available` | modules that are available to load | `module available ANTs` |
| `load` | load an available module | `module load freesurfer` |
| `list` | list all loaded modules | `module list` |
| `purge` | unload all loaded modules | `module purge` |
| `save [collection_name]` | save a named collection of modules | `module save surf` |
| `savelist` | list the names of saved collections<br> (carefull! if you stop at `save`<br> it will do something you don't want| `module savelist` |
| `help [module_name]` | print help about the `module` command or the named module | `module help mrtrix` | 
