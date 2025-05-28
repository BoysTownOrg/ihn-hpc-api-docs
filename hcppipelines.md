## HCPPipelines Tutorial

### Data

Organize your data into BIDS format

```
bids/
├── dataset_description.json
└── sub-001
    └── anat
        ├── sub-001_T1w.json
        ├── sub-001_T1w.nii
        ├── sub-001_T2w.json
        └── sub-001_T2w.nii
```

### Script

Save the following sbatch script as ```hcppipeline.sh```. Edit the paths to the BIDS folder and outputs.

```bash
#!/bin/bash

set -u

export TMPDIR=/ssd/home/$USER/TEMP
participant_label=$1

podman run --rm \
  -v "$HOME":"$HOME" \
  docker.io/bids/hcppipelines:v4.3.0-3 \
    "$HOME"/path/to/bids/ "$HOME"/path/to/outputs participant \
    --participant_label "$participant_label" \
    --license_key idk \ # I don't think this matters...
    --stages PreFreeSurfer FreeSurfer PostFreeSurfer
```

### Run

To run participant sub-001, pass the script and participant label to sbatch from the head node of the HPC

```bash
sbatch hcppipeline.sh 001
```
