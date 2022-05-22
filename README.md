# CropMOSAIKS Modeling Repository

## Purpose

The Modeling repository contains code for utilizing the features created in the [Featurization](https://github.com/cropmosaiks/Featurization) repository, or downloaded from the [MOSAIKS API](https://siml.berkeley.edu). The Random Convolutional Featurization (RCF) process conducted in the Featurization repository produces features that are agnostic to the task a user is interested in modeling. In this repository, the CropMOSAIKS team has focused on using RCF to predict crop yields, but these same features could be used to make predictions of forest cover, population, income, or many other variables visible from space. The key element to building a model is the data which the user needs to supply. This data should be in a standard tabular dataframe, with a spatial component included as column(s) (latitude and longitude) because this data is joined to the feature data spatially. In this repository, the CropMOSAIKS team pairs feature dtaa with crop yield data for the country of Zambia. Due to data availability restrictions, this crop data is not provided with these notebooks. Regardless, the code and documentation in this repository is designed to guide the user in spatially joining the features to the user's data of interest, executing the linear regression step to train the model, applying the trained model to "out-of-bag" data, and statistically analyzing the results.

## Datasets

No feature data or crop data is hosted directly in this repository. To request access to Zambia feature data files, please contact the CropMOSAIKS team (individual GitHub accounts with contact information is included at the end of the main organization README [here](https://github.com/cropmosaiks)). Below we describe the feature data and crop data as it is used in this repository. A user can apply this same workflow to their own data of interest and contact the CropMOSAIKS team with any questions regarding data substitution.

### 1. Features

Random Convolutional Features are a way to encode a geospatial location with information based on the satellite image of that location. These features reflect information such as landscape colors, delimination between colors (like the edge of a field, forest, or building that appears as a line from space), and combinations of colors such as blue next to green. In a feature data frame, each row represents an image, and each feature represents a column. Each cell contains a numerical value for that feature at that location, which is statistically coorelated with the numerical value of crop yield data for that location during the modeling step (or other data provided by the user). Random Convolutional Features can either be created from the featurization repository in this organization, or downloaded from the [MOSAIKS API](https://nadar.gspp.berkeley.edu/home/index/?next=/portal/index/). For more information about featurization and the MOSAIKS pipeline, please see [this paper by Rolf et al. 2021.](https://www.nature.com/articles/s41467-021-24638-z).

### 2. Crop data (labels)

Zambia maize production data is in the form of crop forecast yields in units of metric tonnes/hectare. These units are derived from the expected production reported by the farmers each year in units of metric tonnes, divided by the amount of hectares of farmland planted with maize in that district. This data was collected by the [Central Statistics Office of Zambia (CSO)](https://www.zamstats.gov.zm/). These data are forecasted from survey data collected in May preceding the harvest season (July-August). The forecast model is conducted in Stata, a general purpose statistical software, and adjusted with post-harvest season survey data. There is an unknown degree of uncertainty in this forecast data as the model process and parameters are unknown. Due to data distribution restrictions, this data is not available for download by the public. Any data the user wishes to subsitute should be spatial data associated with the country of Zambia. The spatial resolution of the crop data is at the district-level, but higher resolution data is acceptable and will likely result in improved model perfomance.

### 3. Administrative level boundaries

Administrative boundaries are Zambia district-level boundaries that match the sub-national crop yield data districts. These are used for determining which  points fall within the district boundaries to then summarise the features to the district boundary level. 

### 4. Crop area (weights)

Cropped area in Zambia is used for weighted averages and spatial masking. We spatially mask for crop area in order to optimize our model for crop yields. This data is specific to our task, and other data should be applied if a user is interested in spatially masking for another task. A portion of the cropped area that remains after the mask is applied overlaps with certain cells in our subsetted unform grid. During the modeling process later, the crop land percentage in each cell determines the weight of that area in the model. More cropland in a cell translates to more weight in the model because that land contains more critical feature information. 

## Compute Requirements

Most modern personal computers will be able to run the modeling notebook once access to the user's labeled data is obtained. However, there is a minimum ammount of comfort with Python needed in order to use or adapt this code, including installing Python and managing environments. 

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

There are two primary options to getting started. If you have access to the Master of Environmental Data Science server `taylor.bren.ucsb.edu`, that is the preferred platform. Otherwise, you will need to use personal compute. 

### 1. Using Taylor 
The notebooks are currently configured to be used on Taylor with file paths that lead to persistent data storage with features, administrative boundaries, crop area weights, and the crop yield data. If needed, file paths can be adjusted at the top of the notebook to reflect your data directory location.

### 2. Using Personal Compute
To use personal compute, first clone this repository and configure your environemnt as described above. Following this, adjust the file paths in the top of the document to reflect your data directory location. This repository maintains the structure of the data sub folders and it is recomended to use this structure for your own data. If you are unable to produce your own features, pre-compiled features can be downloaded from the [MOSAIKS API](https://siml.berkeley.edu).

#### Constraints

While most personal computers are able to run this analysis, users may still be constrained by the scope of their analysis (i.e., the number of features, the number of points, and the length of time) that is being modeled. It is possible to expand these variables sufficiently to push a personal computer past its resources. System monitoring may be needed, and it may be neccessary to implement aggresive memory management strategies. For example, a user might want or need to adjust existing code chunks to execute computationally heavy code in smaller steps.

## Notebooks

**Starting notebook & main notebook:** crop_modeling_predicitions_2014_2021.ipynb\
This notebook walks a user through the modeling process from start to finish:
1. Select parameters for the model (satellite, bands, number of points, crop mask, temporal range, etc.)
2. Import, process, and join Zambia's administrative boundary data, feature data, and crop yield data\
  - It is possible and even likely that the user supplied data will be of lower resolution than the feature data. As previously mentioned, the crop data used in this application of the MOSAIKS pipeline is at the district-level.
4. Process NA values (convert infinity values to NA, impute NA values, and drop NA values)
5. Execute visual and statisical data checks along the way
6. Summarize data to the spatial resolution of the crop data (or user-supplied data)
7. Split the data into train and test sets
8. Train the model and visualize results prediction maps
9. Statistically analyze the model's performance with residual maps and various R and R^2 metrics 

#### To be documented by CropMOSAIKS still:
raster clipping
crop land percentage extraction, etc 

## Future Work

As previously discussed, the MOSAIKS pipeline is task-agnostic, meaning that any spatial data of interest can be joined to feature data in order to train a model and make predictions over both time and space. The model's perfomance is measured in general using multiple R and R^2 metrics. Our best model is the model thay results in the highest deameaned R value of 0.61. This value is **not** suspected to be the result of overfitting. The demeaned R^2 value is 0.27, meaning our model explains approximately 27% of the variance in the crop predictions. While this value indicates a relatively strong predictive performance compared to other models that predict over time, there is certainly room for improvement. Suggestions for improving model performance are as follows:\
- Increase the number of training points fed into the model by adding more years of data. This can be acheived by featurizing more years of satellite data. The satellites used in this analysis (Landsat 8 and Sentinel 2) restrict the years and therefore restrict the training N because the lifetime of these satellites starts in 2013 and 2016, respectively. By using different satellites that go further back in time, such as Landsat 5, one can adjust this notebook's code to model over more time. Similarly, if a user has access to more recent years of crop data (2019 and beyond), the number of years and therefore points can be expanded on the more recent end of the timeline.
- Apply a spatial crop mask and densley sample the grid within only the masked regions. In the [Featurization](https://github.com/cropmosaiks/Featurization) repository, CropMOSAIKS applied a grid of ~15,000 (for many parameter combinations) and ~20,000 points (for select parameter combinations) to the country of Zambia, then the user has the choice to apply a crop mask before modeling. By densely sampling the remaining areas after the crop mask has been applied, model performance might improve because there is more data used to train the model and the data is exclusively sampled from regions of the country in which one would expect crops to be grown.
- Test if the maize yield predictions for Zambia prior to 2020 can be used to detect crop yield fluctuations due to known climatic anomalies such as drought. If the crop predictions for these years show significant correlation with precipitation and temperature, this model can be improved upon and used as a tool for governments, community leaders, farmers, and food security initiatives to predict future crop yields for Zambia. This tool was demonstrated by producing predictions for all years used to train the model as well as 2020 and 2021. The ultimate goal is to provide more foresight regarding crop yields prior to harvest, so farmers and leaders can adjust crop imports, exports, and costs. 
- A report presenting a correlational analysis between estimated crop yields and high-resolution, publicly available climate indicators (i.e., temperature and precipitation). This differs from the preceding suggestion because this correlational analysis relates to general temperature and precipitation data, not anomalies. Ideally, this report should be accessible in all dominant languages of Zambia in order to include local farmers and leaders who may not be fluent in English. 
- Stack additional bands to Sentinel 2 in addition to visible spectrum (2,3,4). Examples include short wave infrared (12, 8, and 4), and red edge
- Utilizing notebook for 0.01 degree grid cells (an equal angle grid)
- Increasing the cloud cover limit from 10% to 15% or more
- Filtering cloud cover at the level of the resolution you are featurizing (0.01 degree) rather than at the image level
- Maize is an annual crop and as such, time is not a term in our model. We account for time by grouping features by month and year, which allows us to predict over time. If we included time (i.e. year) as a term in our model, that would mean a single year’s crop yields impact another year’s crop yields. For our goal of predicting annual crop yields, using year as a term in the model is not helpful. However, this would be useful if you are trying to predict for perennial crops such almonds or avocados.

## Contributing

code of conduct, access, API, etc [TC: Also great to acknowledge where things came from, including acknowledging Kathy Baylis and Protensia directly, as well as Caleb and others at PC and the Rolf et al team and the API.]

## TC overall comments
Overall, this README is great but it needs more direction for a user to navigate the repo. What do all these different notebooks do? Which should I start with? How do they relate to one another? What do the subfolders like `subsetting/` and `data/` do? This orientation is a really important component of a README and is currently missing. 
