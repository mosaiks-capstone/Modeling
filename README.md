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
Finally navigate to the diretory with the cloned modeling repo and run 
```bash
jupyter lab
```

## Getting Started

Taylor etc

## Notebooks

It is possible and even likely that the user supplied data will be of lower resolution than the feature data. 
Modeling, rasters

## Constraints

Compute power

## Future Work

suggestions on how to expand

## Contributing

code of conduct, access, API, etc
