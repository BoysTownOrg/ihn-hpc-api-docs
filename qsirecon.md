## submit_qsirecon_test

```
submit_qsirecon_test <image_tag> <script> [args]
```

### Description

Submits a script for execution by a qsirecon container.

### Parameters

- **image_tag**: qsirecon image tag (e.g., 1.0.0)
- **script**: path to script
- **args**: any arguments to forward to **script**

### Example Usage

```bash
#!/bin/bash

source /mnt/apps/scripts/test_slurm_func.sh

LOGDIR=/path/to/logs/
JOB_NAME=QSI
MAX_TIME=24:00:00

for DIR in "$HOME"/qsiprep-output/sub-*/; do
    SUBJECT=$(basename "$DIR")
    submit_qsirecon_test 1.0.0 /path/to/qsirecon.sh "$SUBJECT"
done
```

where qsirecon.sh is

```bash
#!/bin/bash

SUBJECT=$1

qsirecon \
    "$HOME"/qsiprep-output/ \
    "$HOME"/qsirecon-output/ \
    participant \
    --recon-spec dsi_studio_autotrack \
    --fs-license-file /opt/freesurfer/license.txt \
    --participant-label "$SUBJECT"
```

### Notes

- The user's home directory is mounted inside the container and available as **$HOME** within **script**
- **LOGDIR** and **JOB_NAME** must be set before calling **submit_qsirecon_test**
- The freesurfer license is mounted at **/opt/freesurfer/license.txt**
