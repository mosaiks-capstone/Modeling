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

The following is a synopsis of the variables encompassed in the Zambia agricultural survey data, which correspond to the variables that represent the outcomes of interest in the modeling procedure:

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

### 1. Usage on Tsosie
The notebooks are currently configured for use on Tsosie, with file paths that lead to persistent data storage containing features and agricultural data. If necessary, the file paths can be adjusted at the top of the notebook to reflect your data directory location.

### 2. Usage on Personal Compute
To use personal compute, first clone this repository and configure your environemnt as described above. Following this, adjust the file paths at the top of the document to reflect your data directory location. This repository maintains the structure of the data subfolders, and it is recomended to use this structure for your own data. If you are unable to produce your own features, pre-compiled features can be downloaded from the [MOSAIKS API](https://siml.berkeley.edu).

#### Constraints

Although this analysis can generally run on most personal computers, users may still face constraints due to the scope of their analysis, including the number of features, points, and length of time being modeled. These factors can potentially exhaust the resources of a personal computer. Therefore, system monitoring may be necessary, and aggressive memory management strategies may need to be implemented. For instance, users may need to modify existing code chunks to execute computationally heavy code in smaller steps.

## Notebooks and Folders

#### Starting Notebook for Data Preprocessing & Imputation: [insert preprocessing notebook here]
  
This notebook serves as a guide for users to preprocess and impute data in preparation for modeling. The following steps are outlined:
  1. Import, process, and merge feature data and agricultural data. The resolution of the user-supplied data may be lower than that of the feature data. Agricultural data used in this MOSAIKS pipeline application pertains to the SEA-level.
  2. Process NA values (convert infinity values to NA, impute NA values, and drop NA values).
  3. Perform statistical and visual data checks during the process.
  4. Export processed and merged geodataframe to be utilized in subsequent modeling stages. 

### Secondary Notebook for Modeling: [insert modeling notebook here]
  
This notebook serves as a guide for users to model after data preprocessing and imputation:
  
1. Perform a data split into distinct train and test sets to prevent overfitting and ensure unbiased results.
2. Train the model, produce prediction maps, and visually analyze the model's performance.
3. Statistically analyze the model's performance with residual maps and evaluation of metrics such as the R and R^2 coefficients.

#### `Data` folder

This folder and its subfolders are largely placeholders for where to put data when working on a local computer.

## Future Work

As previously stated, the MOSAIKS pipeline is task-agnostic, meaning that it can be applied to any spatial data of interest, allowing for the training of a model and prediction of both temporal and spatial outcomes. The model's perfomance is assessed using various R and R^2 metrics. Our top-performing model obtained the following deameaned R values for the  corresponding variables: 
 [insert best performing values/variables] 
  
These values are not believed to be the result of overfitting. The demeaned R-squared values indicate the degree to which the model explains the variance in the following variables, approximately as percentages:
[insert percentages/variables] 
  
Although these values demonstrate a comparatively robust predictive performance compared to other time-series models, there remains ample opportunity for further refinement. To enhance the training dataset and improve model performance, consider the following recommendations:\
  
- Increase the number of training samples by incorporating more years of satellite data. This can be accomplished by extracting features from additional satellites, such as Landsat 5, which extends further back in time.
- In case of availability of recent agricultural data beyond 2022, incorporating these additional years of data can enhance the dataset and enable the model to capture more recent trends.
  
- Investigate if the agricultural predictions for Zambia prior to 2022 can be used to detect agricultural fluctuations resulting from known climatic anomalies like drought. A thorough evaluation of the model's predictive ability for these years can reveal significant correlations with precipitation and temperature data, further enhancing the tool's accuracy. By achieving this, the model can be utilized by governments, community leaders, farmers, and food security initiatives to anticipate future [insert appropriate] for Zambia. The tool's potential was already demonstrated by generating predictions for all years used to train the model. Ultimately, the goal is to provide more foresight into agricultural yields and indicators before harvest, enabling farmers and leaders to adjust crop imports, exports, and costs accordingly.
  
- A report presenting a correlational analysis between estimated agricultural yields/indicators and high-resolution, publicly available climate indicators (i.e., temperature and precipitation). This report is distinct from the previous suggestion as it focuses on general climate data, rather than anomalies. To ensure accessibility, the report should be made available in all prominent languages of Zambia, to cater to local farmers and leaders who may not be proficient in English.
  
- Stack additional bands to Sentinel 2 in addition to visible spectrum (2,3,4). Examples include short wave infrared (12, 8, and 4), and red edge
 
- Utilizing notebook for 0.01 degree grid cells (an equal angle grid)
  
- Increasing the cloud cover limit from 10% to 15% or more
  
- Filtering cloud cover at the level of the resolution you are featurizing (0.01 degree) rather than at the image level

## Contributing

This project was completed on June 9th, 2023. However, we welcome and encourage suggestions for improvements to the code or documentation. Please submit any questions, comments, or code changes via issues or pull requests on either of the repositories. To communicate with the data scientists responsible for this project, please refer to their personal GitHub accounts listed at the bottom of the organization's README and feel free to contact them via email. You may also contact the authors of the [MOSAIKS paper, Rolf et al. 2021](https://www.nature.com/articles/s41467-021-24638-z) with any questions regarding the process.


The agricultural data used in this MOSAIKS approach for Zambia was generously provided by the [Kathy Baylis lab at UC Santa Barbara](https://baylislab.ace.illinois.edu/), for which Protensia Hadunka offered exceptional support and contextual information on how this model could be helpful to Zambia's governing bodies. It should be noted that this agricultural data is not publicly available. However, this MOSAIKS pipeline and model are generalizable, and can be applied to any data that can be spatially joined with the feature data.

We express our gratitude to the 2022 CropMOSAIKS team for developing the base code that served as the foundation for the MOSAIKS team to extend the MOSAIKS approach. We would also like to thank Cullen Molitor for providing continuous support to the MOSAIKS team throughout the duration of this project.
