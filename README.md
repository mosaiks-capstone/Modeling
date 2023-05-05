# MOSAIKS Modeling Repository

## Purpose

This Modeling repository is the second repository a user should employ, after generating features from satellite imagery through the [Featurization](https://github.com/cropmosaiks/Featurization) repository. This Modeling repository contains code for utilizing the features created in the Featurization repository, or features downloaded from the [MOSAIKS API](https://siml.berkeley.edu). The Random Convolutional Featurization (RCF) process conducted in the Featurization repository produces features that are agnostic to the task a user is interested in modeling. In this repository, the MOSAIKS team has focused on using RCF to predict [insert final interest variables here], but these same features could be used to make predictions on forest cover, population, or many other variables visible from space. The key element to building a model is the data which the user needs to supply. This data should be in a standard tabular dataframe, with a spatial component included as column(s) (latitude and longitude) because this data is joined to the feature data spatially. In this repository, the MOSAIKS team pairs feature data with agricultural survey data for the country of Zambia. Due to data availability restrictions, this agricultural data is not provided with these notebooks. Regardless, the code and documentation in this repository is designed to guide the user in spatially joining the features to the user's data of interest, executing the linear regression step to train the model, applying the trained model to "out-of-bag" data, and statistically analyzing the results.

## Datasets

No feature data or crop data is hosted directly in this repository. To request access to Zambia feature data files, please contact the MOSAIKS team (individual GitHub accounts with contact information is included at the end of the main organization README [here](https://github.com/mosaiks-capstone)). Below we describe the feature data and agricultural data as it is used in this repository. A user can apply this same workflow to their own data of interest and contact the MOSAIKS team with any questions regarding data substitution.

### 1. Features

Random Convolutional Features represent a means of encoding geospatial locations with information based on satellite imagery. These features capture a broad range of detail, such as the color palettes of landscapes, as well as the delineation between colors (e.g., the boundaries between fields, forests, or buildings that are visible from space) and color combinations (e.g., blue next to green). In a feature data frame, each row corresponds to an image, while each column corresponds to a feature. Each cell within the data frame contains a numerical value that corresponds to the feature at that specific location. This value is statistically correlated with the agricultural data employed by the MOSAIKS team or other relevant data provided by the user during the modeling process.

Random Convolutional Features can be generated using either the Featurization repository in this organization or by downloading them from the [MOSAIKS API](https://nadar.gspp.berkeley.edu/home/index/?next=/portal/index/). For further information regarding featurization and the MOSAIKS pipeline, readers are encourages to refer to [this paper by Rolf et al. (2021.)](https://www.nature.com/articles/s41467-021-24638-z).

### 2. Agricultural Data data (labels)

Due to data distribution restrictions, the survey enumeration area (SEA) level agricultural data used in our model is not available for public download. Coarse resolution province-level crop data is available for download from 1987 - 2017 at the [Zambia Data Portal](https://zambia.opendataforafrica.org/).

Here's a reformatted version of the variable list:

| Variable name | Description | Data type |
| --- | --- | --- |
| sea_unq | SEA unique identifier | string |
| year | Date | date |
| total_area_planted_ha | Area planted in hectares | numeric |
| total_area_harv_ha | Total area harvested in hectares | numeric |
| total_area_lost_ha | Total area of crops lost in hectares | numeric |
| total_harv_kg | Total kilograms harvested | numeric |
| yield_kgha | Total kilograms maize per area planted maize | numeric |
| Frac_area_harv | Total area harvested divided by total area planted in hectares | numeric |
| frac_area_loss | Total area lost divided by total area planted in hectares | numeric |
| area_lost_fire | Total area lost due to fire in hectares | numeric |
| maize | Total maize harvested in kilograms | numeric |
| groundnuts | Total groundnuts harvested in kilograms | numeric |
| mixed_beans | Total mixed beans harvested in kilograms | numeric |
| popcorn | Total popcorn harvested in kilograms | numeric |
| sorghum | Total sorghum harvested in kilograms | numeric |
| soybeans | Total soybeans harvested in kilograms | numeric |
| sweet_potatoes | Total sweet potatoes harvested in kilograms | numeric |
| bunding | Total area tilled using bunding in hectares | numeric |
| frac_loss_drought | Drought loss divided by area planted in hectares | numeric |
| frac_loss_flood | Flood loss divided by area planted in hectares | numeric |
| frac_loss_animal | Animal loss divided by area planted in hectares | numeric |
| frac_loss_pests | Pest loss divided by area planted in hectares | numeric |
| frac_loss_soil | Soil loss divided by area planted in hectares | numeric |
| frac_loss_fert | Fertilizer loss divided by area planted in hectares | numeric |
| prop_till_plough | Area ploughed divided by area planted in hectares | numeric |
| prop_till_ridge | Area ridged divided by area planted in hectares | numeric |
| prop_notill | Area not tilled divided by area planted in hectares | numeric |
| prop_hand | Area not hand tilled divided by area planted in hectares | numeric |
| log_maize | Log of total kilograms maize per area maize | numeric |
| log_sweetpotatoes | Log of total kilograms sweet potatoes per area sweet potatoes | numeric |
| log_groundnuts | Log of total kilograms groundnuts per area groundnuts | numeric |
| log_soybeans | Log of total kilograms soybeans per area soybeans | numeric |
| loss_ind | Binary area loss indicator | numeric |
| drought_loss_ind | Binary drought area loss indicator | numeric |
| flood_loss_ind | Binary flood area loss indicator | numeric |
| animal_loss_ind | Binary animal area loss indicator | numeric |
| pest_loss_ind | Binary pest area loss indicator | numeric |
| geometry | Geospatial geometry of polygon | polygon |

This data was collected by the [Central Statistics Office of Zambia (CSO)](https://www.zamstats.gov.zm/). These data are forecasted from survey data collected in May preceding the harvest season (July-August). The forecast model is conducted in Stata, a general purpose statistical software, and adjusted with post-harvest season survey data. There is an unknown degree of uncertainty in this forecast data as the model process and parameters are unknown.  The spatial resolution of the crop data is at the SEA-level, but higher resolution data is acceptable and will likely result in improved model perfomance. 

## Compute Requirements

Most modern personal computers will be able to run the modeling notebook once access to the user's labeled data is obtained. However, there is a minimum ammount of comfort with Python needed in order to use or adapt this code, including installing Python and managing environments. 

<details open>
  <summary> 
    Using personal computer
  </summary>
  
A properly configured python environment can be created through the provided `environment.yml` file. To build this environment open a terminal and run 
```console
conda env create -f environment.yml
```
Then activate the environment with 
```console
conda activate mosaiks
```
Finally open this repository in Jupyter Lab by running 
```console
jupyter lab
```
</details>

<details open>
  <summary> 
    Using <code class="notranslate">taylor.bren.ucsb.edu</code>
  </summary>
  
If a UCSB student in Bren School of Environmental Science & Management using `tsosie.bren.ucsb.edu`, the process to install an environment are more involved but the general principle is the same. The following code should be run one line at a time.
```console
bash                                                   # this will open bash and allow you to navigate directories more easily
cd <dir with environment file>                         # navigate to the directory with this repository clone
conda env create -f environment.yml                    # create new anaconda environment
conda env list                                         # show available environments
conda activate <env name>                              # activate the new environments
conda install ipykernel                                # install ipykernel into new environment
ipython kernel install --user --name=<name_for_kernel> # create and name the kernel
conda deactivate                                       # deactivate environment
```
  
When you open a notebook, the new kernel `<name_for_kernel>` will be available to use.
</details>

## Getting Started

There are two primary options to getting started. If you have access to the Master of Environmental Data Science server `tsosie.bren.ucsb.edu`, that is the preferred platform. Otherwise, you will need to use personal compute. 

### 1. Using Tsosie
The notebooks are currently configured to be used on Tsosie with file paths that lead to persistent data storage with features and agricultural data. If needed, file paths can be adjusted at the top of the notebook to reflect your data directory location.

### 2. Using Personal Compute
To use personal compute, first clone this repository and configure your environemnt as described above. Following this, adjust the file paths in the top of the document to reflect your data directory location. This repository maintains the structure of the data sub folders and it is recomended to use this structure for your own data. If you are unable to produce your own features, pre-compiled features can be downloaded from the [MOSAIKS API](https://siml.berkeley.edu).

#### Constraints

While most personal computers are able to run this analysis, users may still be constrained by the scope of their analysis (i.e., the number of features, the number of points, and the length of time) that is being modeled. It is possible to expand these variables sufficiently to push a personal computer past its resources. System monitoring may be needed, and it may be neccessary to implement aggresive memory management strategies. For example, a user might want or need to adjust existing code chunks to execute computationally heavy code in smaller steps.

## Notebooks and Folders

#### Starting Notebook & Primary Notebook: [insert starting primary notebook here]

This notebook walks a user through the modeling process from start to finish:
1. Select parameters for the model (satellite, bands, number of points, temporal range, etc.)
2. Import, process, and join feature data and agricultural data. It is possible and even likely that the user supplied data will be of lower resolution than the feature data. As previously mentioned, the agricultural data used in this application of the MOSAIKS pipeline is at the SEA-level.
4. Process NA values (convert infinity values to NA, impute NA values, and drop NA values)
5. Execute visual and statisical data checks along the way
6. Summarize data to the spatial resolution of the agricultural data (or user-supplied data)
7. Split the data into train and test sets
8. Train the model and visualize results prediction maps
9. Statistically analyze the model's performance with residual maps and various R and R^2 metrics 

#### `Data` folder

This folder and its subfolders are largely placeholders for where to put data when working on a local computer.

## Future Work

As previously discussed, the MOSAIKS pipeline is task-agnostic, meaning that any spatial data of interest can be joined to feature data in order to train a model and make predictions over both time and space. The model's perfomance is measured in general using multiple R and R^2 metrics. Our best model is the model thay results in the highest deameaned R value of [insert best performing value]. This value is **not** suspected to be the result of overfitting. The demeaned R^2 value is [insert best performing value], meaning our model explains approximately [insert percentage]% of the variance in the [insert best performing variable name]. While this value indicates a relatively strong predictive performance compared to other models that predict over time, there is certainly room for improvement. Suggestions for improving model performance are as follows:\
- Increase the number of training points fed into the model by adding more years of data. This can be acheived by featurizing more years of satellite data. The satellite used in this analysis (Sentinel 2) restrict the years and therefore restrict the training N because the lifetime of these satellites starts in 2015. By using different satellites that go further back in time, such as Landsat 5, one can adjust this notebook's code to model over more time. Similarly, if a user has access to more recent years of agricultural data (2022 and beyond), the number of years and therefore points can be expanded on the more recent end of the timeline.
- Test if the agricultural predictions for Zambia prior to 2022 can be used to detect agricultural fluctuations due to known climatic anomalies such as drought. If the predictions for these years show significant correlation with precipitation and temperature, this model can be improved upon and used as a tool for governments, community leaders, farmers, and food security initiatives to predict future [insert appropriate] for Zambia. This tool was demonstrated by producing predictions for all years used to train the model. The ultimate goal is to provide more foresight regarding agricultural yields and indicators prior to harvest, so farmers and leaders can adjust crop imports, exports, and costs. 
- A report presenting a correlational analysis between estimated agricultural yields/indicators and high-resolution, publicly available climate indicators (i.e., temperature and precipitation). This differs from the preceding suggestion because this correlational analysis relates to general temperature and precipitation data, not anomalies. Ideally, this report should be accessible in all dominant languages of Zambia in order to include local farmers and leaders who may not be fluent in English. 
- Stack additional bands to Sentinel 2 in addition to visible spectrum (2,3,4). Examples include short wave infrared (12, 8, and 4), and red edge
- Utilizing notebook for 0.01 degree grid cells (an equal angle grid)
- Increasing the cloud cover limit from 10% to 15% or more
- Filtering cloud cover at the level of the resolution you are featurizing (0.01 degree) rather than at the image level

## Contributing

This project was completed on June 9th, 2023, but suggestions for improvements to the code or documentation is welcome and encouraged. Please submit questions, comments, or code via issues or pull requests on either of the repositories. To correspond with the data scientists who produced these materials that extend the MOSAIKS approach, please see their personal GitHub accounts at the bottom of the organization's README and feel free to contact them via email. Additionally, you can contact the authors of the [MOSAIKS paper, Rolf et al. 2021](https://www.nature.com/articles/s41467-021-24638-z) with questions about the process.

The crop yield data for Zambia used in this application of the MOSAIKS approach was generously provided by the [Kathy Baylis lab at UC Santa Barbara](https://baylislab.ace.illinois.edu/), and Protensia Hudunka was exceptionally helpful in providing metadata and contextual information regarding how this model might be helpful to the governing bodies of Zambia. This crop data is not available to the public. However, this overall pipeline and model is generalizable and therefore can be used on any data that can be spatially joined the feature data.

We would like to thank the members of the 2022 CropMOSAIKS team, that wrote much of the base code for this repository that provided the foundation for the MOSAIKS team to extend the MOSAIKS approach. We would like to extend a special thanks to Cullen Molitor, who offered the MOSAIKS team support throughout the duration of this project.
