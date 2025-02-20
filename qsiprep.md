## submit_qsiprep_test

```
submit_qsiprep_test <image_tag> <script> [args]
```

### Description

Submits a script for execution by a qsiprep container.

### Parameters

- **image_tag**: qsiprep image tag (e.g., 1.0.0)
- **script**: path to script
- **args**: any arguments to forward to **script**

### Example Usage

```bash
#!/bin/bash

source /mnt/apps/scripts/test_slurm_func.sh

LOGDIR=/path/to/logs/
JOB_NAME=QSI
MAX_TIME='24:00:00'

for DIR in "$HOME"/qsiprep_runs/BIDS/sub-*/; do
    SUBJECT=$(basename "$DIR")
    submit_qsiprep_test 1.0.0 /path/to/qsiprep.sh "$SUBJECT"
done
```

where qsiprep.sh is

```bash
#!/bin/bash

SUBJECT=$1

qsiprep \
    "$HOME"/qsiprep_runs/BIDS/ \
    "$HOME"/qsiprep_runs/post_qsiprep/ \
    participant \
    --output-resolution 2 \
    --fs-license-file /opt/freesurfer/license.txt \
    --participant-label "$SUBJECT"
```

### Notes

- The user's home directory is mounted inside the container and available as **$HOME** within **script**
- **LOGDIR** and **JOB_NAME** must be set before calling **submit_qsiprep_test**
- The freesurfer license is mounted at **/opt/freesurfer/license.txt**
