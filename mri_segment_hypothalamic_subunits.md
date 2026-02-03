## mri\_segment\_hypothalamic\_subunits Tutorial

### Data

Create a text file, `sd.txt`, in HPCHome containing the path to the recon-all subject directory relative to HPCHome:

```
recon-all-output/
```

### Script

Save the following script as `mri_segment_hypothalamic_subunits-job.sh` in HPCHome:

```bash
#!/bin/bash
set -eu

subject_dir=$1
mri_segment_hypothalamic_subunits --s --sd "$subject_dir" --write_posteriors --threads 10 --cpu
```

### Run

Start the job:

```bash
/mnt/home/shared/bashford/ihn-hpc-sbatch-array freesurfer --tag 8.1.0 mri_segment_hypothalamic_subunits-job.sh sd.txt --sbatch-args "--cpus-per-task=10"
```

You can adjust the `--cpus-per-task` option but will most likely want it to match the prior `--threads` option.
