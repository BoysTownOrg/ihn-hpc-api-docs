## recon-all Tutorial

### Data

Create a text file, `niis.txt`, in HPCHome containing the paths to each NIfTI relative to HPCHome. Put each path on a separate line, e.g.

```
data/M68111111.nii
data/M68222222.nii
data/M68333333.nii
```

For this tutorial we assume an URSI, e.g. `M68111111`, is embedded in each path, and the embedded URSI will be the subject identifier.

### Script

Save the following script as `recon-job.sh` in HPCHome:

```bash
#!/bin/bash
set -eu

nii=$1
subject_id=$(echo "$nii" | grep -Eo "(m|M)[0-9]{8}" | head -n 1)
export FS_V8_XOPTS=0
recon-all -i "$HPC_HOME/$nii" -sd "$HPC_HOME"/recon-all-output/ -subjid "$subject_id" -all
```

### Run

Create the output folder in your HPCHome:

```bash
cd
mkdir recon-all-output
```

Start the jobs:

```bash
/mnt/home/shared/bashford/ihn-hpc-sbatch-array freesurfer --tag 8.1.0 recon-job.sh niis.txt --max-tasks 100
```

You may adjust `--max-tasks` based on how many NIfTIs you are processing. This number represents the maximum number of recon-all jobs that will run simultaneously. Try not to use more than ~120 to be polite to other users.
