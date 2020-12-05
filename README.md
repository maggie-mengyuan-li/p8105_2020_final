# Factors Influencing Abortion Rates in the US

Welcome to the github project repository for the Fall 2020 P8105 Final Project, **Factors Influencing Abortion Rates in the US**, by Nancy Fang, Maggie Li, Rachel Tao, and Linh Tran. 

### Project Overview

To navigate to our project website, please visit:

**Website Link:** https://maggie-mengyuan-li.github.io/p8105_2020_final/index.html

### Data

We downloaded state-level data on abortions, contraceptive access, and birth outcomes directly from the [Guttmacher Institute Data Center (2014-2017)](https://data.guttmacher.org/states). We then manually extracted state-level data on hostility around abortion rights provided by the [Guttmacher Institute](https://www.guttmacher.org/sites/default/files/article_files/hostile_and_supportive_state_abortion_laws_1-24-2020.pdf).

These raw files can be found in our data folder, and we go over the cleaning and tidying of these data in our full report.

We also downloaded a publicly-available US States shapefile from the [US Census Bureau](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html) for our spatial visualizations, which can also be found in our data folder. 

### Needed Packages

Please be sure to download the following packages to ensure that you can knit the R Markdown files in our repository:  

`install.packages("tidyverse")`  
`install.packages("readr")`  
`install.packages("sf")`    
`install.packages("rgdal")`  
`install.packages("huxtable")`   
`install.packages("patchwork")`   
`install.packages("broom")`
`install.packages("plotly")`  
`install.packages("devtools")`   
`install.packages("modelr")`   
`install.packages("mgcv")`
