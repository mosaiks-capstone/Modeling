# CropMOSAIKS Modeling Repository

## Purpose

The Modeling repository contains code for utilizing the features created in the Featurization repository, or downloaded from the [MOSAIKS API](https://nadar.gspp.berkeley.edu/home/index/?next=/portal/index/). Features created through Random Convolutional Features leave them agnostic to the task a user is interested in modeling. The CropMOSAIKS team have focused on modeling crop data, but these same features could be used to make prediction on forest cover, population, income, and many other variables. The key element to building a model is the data which the user needs to supply.

## Datasets

Features, administrative level boundaries, crop area, crop data

## Requirements

A properly configured python environment can be created through the provided `environment.yml` file. To build this environment open a terminal and run 
```bash
conda env create -f environment.yml
```
Then activate the environment with 
```bash
conda activate mosaiks
```
Finally open this repository in Jupyter Lab by running 
```bash
jupyter lab
```

## Getting Started

There are two primary option to getting started, connecting to the MEDS server `taylor.bren.ucsb.edu`, or using personal compute. 

### Taylor
The notebooks are currently configured to be used on Taylor with file paths that lead to persistent data storage with features, administrative boundaries, crop area weights, and the crop yield data. 

### Personal Compute
To use personal compute, first clone this repository and configure your environemnt as described above. Following this, adjust the file paths in the top of the document to reflect your data directory location. This repository maintains the structure of the data sub folders and it is recomended to use this structure for your own data. If you are unable to produce your own features, pre-compiled features can be downloaded from the [MOSAIKS API](https://nadar.gspp.berkeley.edu/home/index/?next=/portal/index/).

## Notebooks

It is possible and even likely that the user supplied data will be of lower resolution than the feature data. 
Modeling, rasters

## Constraints

Compute power

## Future Work

suggestions on how to expand

## Contributing

code of conduct, access, API, etc
