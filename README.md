# Material for the nextsim workshop

This repo contains instructions on how to run nextsim tutorials presented at the nextsim workshop.

You need 3 ingredients :
  - the [notebooks](#notebooks)
  - the [data](#data)
  - the [computing environment](#computing-environment)


## Prerequisites

You will need docker on your machine, if you do not have it already you can install it from here : https://docs.docker.com/get-started/get-docker/
You will also need around 30Gb of storage for the data and the docker image

## First step

We suggest that you create a dedicated repository for this workshop that will contain all the ingredients : ```mkdir nextsim-workshop```

## Notebooks

The notebooks are stored in this very repo, download them with : `git clone https://github.com/sasip-climate/nextsim-workshop2025.git`

## Data

The minimal dataset needed to run each examples can be downloaded with `wget https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/fileServer/meomopendap/extract/SASIP/data-nextsim-workshop2025.tar` and then untar with `tar -xvf data-nextsim-workshop2025.tar`

## Computing environment

The docker image containing the python libraries and a compiled version of nextsimdg can be downloaded with the command :

```bash
docker run --rm -v /YOURPATH/nextsim-workshop:/home/nextsim-workshop -p 8888:8888 quay.io/auraoupa/nextsim-workshop:615073e1b4f2
```

where `YOURPATH` must be replaced by the absolute path on your laptop leading to the `nextsim-workshop` directory

A jupyterlab is now deployed, you just have to open in a browser the given adress `http://127.0.0.1:8888/lab?token=...` with your assigned token

## Results

You should have this tree of directories on your laptop :

```
YOURPATH/nextsim-workshop
├── nextsim-workshop2025 (git repo)
│   └── notebooks
|       ├── assimiliation
│       ├── nextsimdg
│       └── tuto
├── data-nextsim-workshop2025 (wget)
│   ├── assimiliation
│   ├── nextsimdg
│   └── tuto
```

And the mirror of it on jupyterlab, with extra repositories `NEDAS` and `nextsimdg` at the same level than `nextsim-workshop`

