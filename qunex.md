## Exploring Qunex

### Trying to get HCPPipelines to work

Organize NIIs into a BIDS folder.

Save the following sbatch script as ```qunex.sh```.

```bash
#!/bin/bash

set -u

export TMPDIR=/ssd/home/$USER/TEMP

podman run --rm \
  -v "$HOME":"$HOME" \
  --env HOME \
  docker.io/qunex/qunex_suite:1.0.4 "$HOME"/hack.sh
```

Save the following script as ```hack.sh```. Edit the path to the BIDS folder.

```bash
#!/bin/bash

bids_folder="$HOME"/bids/
study_folder="$HOME"/qunex-study/

source /opt/qunex/env/qunex_environment.sh

# Refer to https://qunex.readthedocs.io/en/latest/wiki/Overview-QuickStart.html#execution-of-commands
qunex create_study --studyfolder="$study_folder"
qunex import_bids --sessionsfolder="$study_folder"/sessions/ --inbox="$bids_folder"
# The next line replaces create_session_info. Hack.
cp "$study_folder"/sessions/001/session.txt "$study_folder"/sessions/001/session_hcp.txt
qunex create_batch --sessionsfolder="$study_folder"/sessions/
qunex setup_hcp --sessionsfolder="$study_folder"/sessions/ --batchfile="$study_folder"/processing/batch.txt --check=no
qunex hcp_pre_freesurfer --sessionsfolder="$study_folder"/sessions/ --batchfile="$study_folder"/processing/batch.txt
qunex hcp_freesurfer --sessionsfolder="$study_folder"/sessions/ --batchfile="$study_folder"/processing/batch.txt
qunex hcp_post_freesurfer --sessionsfolder="$study_folder"/sessions/ --batchfile="$study_folder"/processing/batch.txt
```

To run:

```bash
sbatch qunex.sh
```

Notes:
- I was not able to create the ["session info"](https://qunex.readthedocs.io/en/latest/wiki/UsageDocs-HCPMappingDocumentation.html#hcp-session-pipeline-information-file-session-hcp-txt) file using the create_session_info command, but the ["session"](https://qunex.readthedocs.io/en/latest/wiki/UsageDocs-HCPMappingDocumentation.html#the-qunex-session-file-session-txt) file looked like it was close enough, so I just copied it (see above). The session, e.g. 001, will need to be edited. 
