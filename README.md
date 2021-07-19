# colvilleConnectivity

## Scripts
### Google Earth Engine - links to each file
1. Download RGB data for lakes and channel apex: [https://code.earthengine.google.com/73c33750b9f646df31b761aa50776e9d]
2. Download lake ice data [https://code.earthengine.google.com/11757ed73298fa104aed9c5d5b86f5b6] - by Xiao Yang

### R scripts --descriptions of each file. Files are included separately on github
1. 1_ColvilleDeltaClassification_updateFeb2021.Rmd
  - **Description**: Intakes lake and channel reflectance data and classifies lakes as high functional connectivity vs low functional connectivity within multiple time periods	
  - **input files**
    - ColvilleDataExport_20210210_75pctCloud.csv:Lake reflectance data - from google earth engine script no. 1, also included in data repository
        - ID: Lake ID
        - system_time_start_mean: The time of the observation--values need to be divided by 1000 before hey can be converted to formal dates
        - delta: name of the delta--should always be "colville"
        - Blue_mean, Blue_p10, Blue_p90: Blue reflectance for a lake on a given date (either mean, 10%, 90%)--values need to be divided by 1000 to be scaled
        - Green_mean, Grean_p10, Green_p90: same as above, for Green reflectance--values need to be divided by 1000 to be scaled
        - Red_mean, Red_p10, Red_p90: same as above, for Red reflectance--values need to be divided by 1000 to be scaled
        - Gb_ratio_mean, Gb_ratio_p10, Gb_ratio_p90, same as above for Green/Blue reflectance
        - Ndssi_mean, Ndssi_p10, Ndssi_p90: same as above, for NDSSI (blue-nir)/(blue+nir)
        - Nsmi_mean, Nsmi_p10, Nsmi_p90: same as above, for NSMI (red+green-blue)/(red+green+blue)
        - area_m: lake area in m^2
        - count: Total number of pekel 90% occurence water pixels within the lake boundary
        - Red_count: Total number of cloud free pixels included in the mean,p90, p10 values for all color reflectances
    - ColvilleApexDataExport_20210210_75pctCloud.csv: Channel reflectance data - from google earth engine script no.1, also included in data repository
     	- Same as above file, except no ID or delta column. 
    - ColvilleShapefilesEdited.shp: The lake polygon shapefiles, included in data repository
    - colvilleValidation20200508.csv: Connectivity validation data from GCI imagery
        - Row.Number: Index
        - ID: Lake ID
        - Connected: "y" channel present, "m" uncertain channel presence, "n" no channel presence
        - note: any notes made during analysis
2. 
