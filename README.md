# colvilleConnectivity

## Scripts

### Data download
1. Download RGB data for lakes and channel apex: [https://code.earthengine.google.com/73c33750b9f646df31b761aa50776e9d]
2. Download lake ice data [https://code.earthengine.google.com/11757ed73298fa104aed9c5d5b86f5b6] - by Xiao Yang

### Preprocessing
1. Piliouras R
2. Piliouras GEE
3. ice analysis R

### Main data processing -- all input files are included in the data repository.
#### R scripts --descriptions of each script. 
- 1_ColvilleDeltaClassification_updateJuly2021.Rmd
  - **Description**: Intakes lake and channel reflectance data and classifies lakes as high functional connectivity vs low functional connectivity within multiple time periods	
  - **input files**
    - ColvilleDataExport_20210210_75pctCloud.csv: Lake reflectance data - from google earth engine script no. 1
    - ColvilleApexDataExport_20210210_75pctCloud.csv: Channel reflectance data - from google earth engine script no.1
    - ColvilleShapefilesEdited.shp: The lake polygon shapefiles
    - colvilleValidation20200508.csv: Connectivity validation data from GECI imagery
  - **output files**
    - Colville_Classified_data_20210303: The results of the functional connectivity classification for each lake within each time period
    - densityPlotExample.pdf: A pdf of the density plot portion of Figure 2.
    - clusteringExample.pdf: A pdf of Figure 3---clustering example
- 2_ColvilleResultes.Rmd
  - **Description**: Uses connectivity classifications to: validate classification methods, study how connectivity has changed over time in the Colville Delta, and analyze the relationship between connectivity and elevation, discharge, and lake ice
  - **input files**
    - Colville_Classified_data_20210303: The results of the functional connectivity classification for each lake within each time period
    - ColvilleShapefilesEdited.shp: The lake polygon shapefiles
    - colvilleValidation20200508.csv: Connectivity validation data from GECI imagery
    - colville1992Classification.csv: Connectivity validation data from Jorgenson et al. (1997) -----------------******
    - PilouriasColvilleClassCompareNegBuf_20200520_good.shp: Connectivity validation data from Piliouras and Rowland (2020) *****
    - AprilLakeElev20152017.csv: Mean April lake elevation in 2015 and 2017 from ArcticDEM strip data. -----------******
    - lake_ice_modeled_duration_colville_delta_20210304.RData: Modeled lake ice phenology from Landsat -------------*****
    - NumberOfObsTrans_20210304.RData: number of ice observations from Landsat for each lake within both the breakup and freeze-up periods -------------******
  - **output files**
    - alaskaMap.pdf: Figure 1 part 1 (map of Alaska)
    - deltaMap.pdf: Figure 1 part 2 (map of the Colville Delta)
    - validationFig.pdf: Figure 4 (validation of connectivity algorithm against three datasets)
    - timeResultsFigure_map.pdf: The map portion of Figure 5 (connectivity through time)
    - timeResultsFigure_plot.pdf: The barplot portion of Figure 5 (connectivity through time)
    - dischargeBarPlots.pdf: The barplot portion of Figure 6 (comparing connectivity and discharge)
    - dischargeTimeSeries.pdf: The lineplot portion of Figure 6 (comparing connectivity and discharge)
    - elevFig.pdf: Figure 7 (boxplot comparing connectivity and lake surface elevation)
    - iceFigBarPlot.pdf: Figure 8 (comparison of functional connectivity to ice phenology)
    - iceNoObsPlot.pdf: Figure 9 (number of ice observations used in ice phenology calculations)

### File variable descriptions for files included in the data repository
   - ColvilleDataExport_20210210_75pctCloud.csv: Lake reflectance data - from google earth engine script no. 1, also included in data repository
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
      - ID: Lake ID
      - delta: name of the delta--should always be 'colville'
      - type: 'delta'--should always be 'delta'
      - geometry: the geometry of the lake polygon
   - colvilleValidation20200508.csv: Connectivity validation data from GECI imagery, included in data repository
        - Row.Number: Index
        - ID: Lake ID
        - Connected: "y" channel present, "m" uncertain channel presence, "n" no channel presence
        - note: any notes made during analysis
   - Colville_Classified_data_20210303: The results of the functional connectivity classification for each lake within each time period, included in data repository
        - ID: Lake ID
        - time_period: The time period for the classification
        - data: a nested dataframe containing the dominant wavelength data used in the classification
        - class: The number of the class that the lake was sorted into in the specified time period.
        - connectivity: The classified functional connectivity of the lake within the specified time period
        - type: validation classification from the GECI data set, "y" channel present, "n" no channel present, "m" uncertain channel presence
   - colville1992Classification.csv: Connectivity validation data from Jorgenson et al. (1997)
        - Row.Number: Index
        - ID: Lake ID
        - Connected: Connectivity classification from GECI
        - MapClassification: Connectivity Classification from Jorgenson et al. (1997)
   - PilouriasColvilleClassCompareNegBuf_20200520_good.shp: Connectivity validation data from Piliouras and Rowland (2020) 
        - delta: Name of the delta--should always be "colville"
        - count: number of 'connected' pixels within each lake
        - ID: Lake ID
        - geometry: lake polygons with a negative 30m buffer. 
   - AprilLakeElev20152017.csv: Mean April lake elevation in 2015 and 2017 from ArcticDEM strip data. 
        - ID: Lake ID
        - area_m: lake area in m^2
        - count: Total number of pekel 90% occurence water pixels within the lake boundary
        - delta: Name of the delta-- should always be "colville"
        - elev2015_mean: mean April 2015 lake elevation (m)
        - elev2015_median: median April 2015 lake elevation (m)
        - elev2017_mean: mean April 2017 lake elevation (m)
        - elev2017_median: median April 2017 lake elevation (m)
   - lake_ice_modeled_duration_colville_delta_20210304.RData: Modeled lake ice phenology from Landsat 
        - ID: Lake ID
        - ice_duration: modeled total ice duration (days)
        - ice_free_duration: modeled ice free duration (days)
        - total_transition_duration: modeled total ice transition (breakup and freeze-up periods) duration (days)
        - bu_transition_duration: modeled breakup transition duration from breakup start to breakup end (days)
        - fu_transition_duration: modeled freeze-up transition duration from freeze-up start to freeze-up end (days)
        - buStart: Breakup start--modeled first day of year with ice fraction <80% (day of year)
        - buEnd: Breakup end--modeled first day of year with ice fraction <20% (day of year)
        - fuStart: Freeze-up start--modeled first day of fall with ice fraction >20% (day of year)
        - fuEnd: Freeze-up end--modeled first day of fall/winter with ice fraction >80% (day of year)
   - NumberOfObsTrans_20210304.RData: number of ice observations from Landsat for each lake within both the breakup and freeze-up periods 
        - ID: Lake ID
        - count_bu: number of ice observations used during the modeled breakup period
        - count_fu: number of ice observations used during the modeled freeze-up period
