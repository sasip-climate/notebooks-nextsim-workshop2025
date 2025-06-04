![image](snapshot36BBM.png)

_2025 SASIP Workshop_
#### Data exploration session:
## Kilometer-scale sea-ice-ocean coupled simulations with a brittle rheology
Stephanie Leroux (Datlas, Grenoble), Pierre Rampal (IGE/CNRS, Grenoble), Laurent Brodeau (IGE/CNRS, Grenoble)

### Session teaser:
In this session of the workshop we will explore a dataset made of  sea-ice-ocean  simulations with the SI3-NEMO model at 1/12° (4 km) and 1/36° (1 km) horizontal  resolution  produced by Laurent Brodeau and Pierre Rampal at IGE Grenoble. The Elasto-Brittle rheology (BBM) has been implemented in the SI3 sea ice model (Brodeau et al [2024 https://doi.org/10.5194/gmd-17-6051-2024](https://doi.org/10.5194/gmd-17-6051-2024)) in the Eulerian framework, based on a staggered grid (" E grid"). In the  SASIP project, we use this framework to test the BBM rheology at high-reslution (kilometric scale) resolution and with ocean coupling.

-> In this session, we will primarily look all together at some videos of those simulations produced at 1/36° (1 km) and 1/12° (4 km) with BBM rheology as well as with the standard aEVP rheology available in this model. 

-> Workshop participants will also have access to the model outputs at 1/12° resolution so that they can  look at more specific diagnostics of their own during the following __hackathon session (see our suggestions of possible hackathon projects below)__.

### VIDEOS:
* The videos that we will discuss in this session are available in this [YouTube playlist](https://www.youtube.com/playlist?list=PLvzG0ke9xnX6fLPuoiMdLpQa0SM7Mvujb). Only one video has been  uploaded so far but some more will be uploaded before the workshop.

### MODEL OUTPUTS DOWNLOAD:
* Model outputs will be available as netcdf files with this [OpenDap link](https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/catalog/meomopendap/extract/SASIP/extra-data-nextsim-workshop2025/hires-BBM/catalog.html) by the day of the workshop.
* We will also provide some lagrangian trajectories of sea ice drifters computed from the simulations,  initialized every 10 days at the locations of observed sea ice buoys from the [IABP observation program](https://iabp.apl.uw.edu/) during the simulated period (winder 1997). Those trajectories will be available on the same link as above.

To download any of the files with wget from the terminal (adjust the name of the file you wish to download):
```
 wget https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/fileServer/meomopendap/extract/SASIP/extra-data-nextsim-workshop2025/hires-BBM/your-file-to-download.nc
```

### Suggestions for the HACKATHON session:
Some suggestions for your hackathon project:
1. Download and explore the lagrangian trajectories provided from our 1/12° sea ice simulations. It is a light dataset and you will be able to compare the simulated trajectories from our simulations with the two types of rheology (BBM and aEVP), and with the real observed trajectories from the [IABP observation program](https://iabp.apl.uw.edu/).
2. Download the model gridded outputs to apply some diagnostics of your own on the sea ice fields available hourly or every 3h.

Then don't forget to list your project idea in the [workshop sheet [here]](https://docs.google.com/spreadsheets/d/1OgvTLPxvWeuxptNd_5-aiuCvT09M2M-Gbqnf7QnHIq0/edit?usp=sharing).
