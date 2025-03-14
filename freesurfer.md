## submit_generic_freesurfer

```
submit_generic_freesurfer <script> [args]
```

### Description

Submits a script for execution by a freesurfer container.

### Parameters

- **script**: path to script
- **args**: any arguments to forward to **script**

### Example Usage

```bash
#!/bin/bash

source /mnt/apps/scripts/slurm_func.sh

LOGDIR=/path/to/logs/
JOB_NAME=recon-all
MAX_TIME=24:00:00

shopt -s globstar
for NII in "$HOME"/recon-input/**/t1*.nii.gz; do
    submit_generic_freesurfer /path/to/recon-all.sh "$NII"
done
```

where recon-all.sh is

```bash
#!/bin/bash

NII=$1
# Assume NIfTI has URSI (e.g. M68123456) within path
URSI=$(echo "$NII" | grep -Eo "(m|M)68[0-9]{6}")
recon-all -i "$NII" -sd "$HOME"/recon-output/ -subjid "$URSI" -all -openmp 4
```

### Notes

- The user's home directory is mounted inside the container and available as **$HOME** within **script**
- **LOGDIR** and **JOB_NAME** must be set before calling **submit_generic_freesurfer**
