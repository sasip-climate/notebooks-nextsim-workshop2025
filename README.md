# Material for the nextsim workshop

This repo contains instructions on how to run nextsim tutorials presented at the nextsim workshop.

You need 3 ingredients :
  - the computing environment : a docker image can be downloaded from quay.io with the command `docker pull xxx`
  - the data : the minimal dataset needed to run each examples can be downloaded with `wget yyyy` and then untar with `tar -xvf yy`
  - the notebooks : they are stored in this very repo, download them next to the data with : `git clone https://github.com/sasip-climate/nextsim-workshop2025.git`

These operations can be done on your laptop or any clusters that allow a docker deployment, you r workspace should be organized like this :

```
YOURHOME/OR/WORKDIR
├── nextsim-workshop2025 (git repo)
│   └── notebooks
|       ├── data-assimiliation-nextsimdg
│       ├── nextsimdg
│       └── tuto
├── data-nextsim-workshop2025 (wget)
│   ├── data-assimiliation-nextsimdg
│   ├── nextsimdg
│   └── tuto
```

