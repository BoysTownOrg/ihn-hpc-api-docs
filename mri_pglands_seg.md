## mri\_segment\_hypothalamic\_subunits Tutorial

### Patch

Download the patched [mri_pglands_seg](https://raw.githubusercontent.com/freesurfer/freesurfer/refs/heads/dev/mri_pglands_seg/mri_pglands_seg) and save in HPCHome.

### Script

Save the following script as `mri_pglands_seg.sh` in HPCHome. Edit the path to the recon-all-output directory as needed.

```bash
#!/bin/bash

set -eu

echo y | "$FREESURFER_HOME"/bin/fs_install_cuda
mri_pglands_seg --s --sd "$HPC_HOME"/recon-all-output/ --write_vol_stats --write_qa_stats --use_cuda
```

### Run

Start the job:

```bash
/mnt/home/shared/bashford/ihn-hpc-sbatch-gpu freesurfer --tag 8.1.0 mri_pglands_seg.sh --sbatch-args "--cpus-per-task=10" --podman-args "-v $HOME/mri_pglands_seg:/usr/local/freesurfer/8.1.0-1/python/scripts/mri_pglands_seg"
```
