# colvilleConnectivity

These scripts and data are public for a manuscript currently under review. 

## Data files

All datafiles mentioned below can be found on Zenodo: http://doi.org/10.5281/zenodo.5115099

## Scripts

### Data download to google drive
1. Download RGB data for lakes and nearby channels: [https://code.earthengine.google.com/66cc087785402ba82d27d49c648a3808]
2. Download lake ice data [https://code.earthengine.google.com/11757ed73298fa104aed9c5d5b86f5b6] - by Xiao Yang
3. Download of April 2015 and April 2017 lake elevations from ArcticDEM [https://code.earthengine.google.com/73182eaa7b7bf0aa15959dba44bde194]

### Pre-processing -- input and output files are included in data repository
- Delta lake ice.Rmd - By Xiao Yang, some edits by Wayana Dolan
   - **description**: Using downloaded ice fraction data to model 20-year lake ice phenology
   - **inputs**:
      - lakeCoverFraction_1b786b4795b34fa035d55c102cc305e7.csv: Output from step 2 of data download. 
   - **outputs**:
      - lake_ice_modeled_duration_colville_delta_20210304.RData: Modeled lake ice phenology from Landsat 
      - NumberOfObsTrans_20210304.RData: number of ice observations from Landsat for each lake within both the breakup and freeze-up periods

### Main data processing -- all input files are included in the data repository as are all output files, excluding figure pdfs.
- 1_ColvilleDeltaClassification_updateJuly2021.Rmd
  - **Description**: Intakes lake and channel reflectance data and classifies lakes as high functional connectivity vs low functional connectivity within multiple time periods and using different methods of classificatioj	
  - **input files**
    - ColvilleDataExport_20211022.csv: Lake reflectance data - from google earth engine script no. 1
    - ColvilleChannelExport_1km_20211022.csv, ColvilleChannelExport_2km_20211022.csv", ColvilleChannelExport_5km_20211022.csv", ColvilleChannelExport_10km_20211022.csv: Channel reflectance data from within 1-10km of each lake. From google earth engine script no. 1
    - ColvilleShapefilesEdited.shp: The lake polygon shapefiles
    - colvilleValidation20200508.csv: Connectivity validation data from GECI imagery
  - **output files**
    - colville_dt_20211022: lake connectivity classification results using a decision tree-based approach (the final approach used in the paper analysis)
    - colville_rf_20211022: lake connectivity classification results using a random forest-based approach
    - colville_pct_20211022: lake connectivity classification results using a percentage-based thresholding technique
    - colville_km_20211022: lake connectivity classification results using a k-means-based classification approach
    - densityPlotExample.pdf: A pdf of the density plot portion of Figure 2.
    - DecisionTreeFigure.pdf: A pdf of the final decision tree (Figure 3).

- 2_ColvilleResultes_updateJuly2021.Rmd
  - **Description**: Uses connectivity classifications to: validate classification methods, study how connectivity has changed over time in the Colville Delta, and analyze the relationship between connectivity and elevation, discharge, and lake ice
  - **input files**
    - colville_dt_20211022: lake connectivity classification results using a decision tree-based approach (the final approach used in the analysis)
    - colville_rf_20211022: lake connectivity classification results using a random forest-based approach
    - colville_pct_20211022: lake connectivity classification results using a percentage-based thresholding technique
    - colville_km_20211022: lake connectivity classification results using a k-means-based classification approach
    - ColvilleShapefilesEdited.shp: The lake polygon shapefiles
    - colvilleValidation20200508.csv: Connectivity validation data from GECI imagery
    - colville1992Classification.csv: Connectivity validation data from Jorgenson et al. (1997)
    - PilouriasColvilleClassCompareNegBuf_20200520_good.shp: Connectivity validation data from Piliouras and Rowland (2020) 
    - AprilLakeElev20152017.csv: Mean April lake elevation in 2015 and 2017 from ArcticDEM strip data
    - lake_ice_modeled_duration_colville_delta_20210304.RData: Modeled lake ice phenology from Landsat (summary statistics for ice phenology dates)
    - lake_ice_modeled_20210304.RData: Modeled lake ice phenology from Landsat (daily modeled ice fraction data)
    - NumberOfObsTrans_20210304.RData: Number of ice observations from Landsat for each lake within both the breakup and freeze-up periods
    - 150_Alaska_nolakes_widths.shp: Alaskan rivers from the GRWL Summary Statistics database (Allen & Pavelsky, 2018) that are wider than 150m.
  - **output files**
    - alaskaMap.pdf: Figure 1 part 1 - map of Alaska
    - deltaMap.pdf: Figure 1 part 2 - map of the Colville Delta
    - validationFig.pdf: Figure 5 - comparison of connectivity algorithm against three datasets
    - timeResultsFigure_map.pdf: The map portion of Figure S1 (connectivity through time)
    - timeResultsFigure_plot.pdf: The barplot portion of Figure S1 (connectivity through time)
    - temporalSummary_plot.pdf: Figure 6 - connectivity over time
    - dischargeBarPlots.pdf: Figure 7 - comparing connectivity within different discharge groups
    - elevFig.pdf: Figure 8 - boxplot comparing connectivity and lake surface elevation
    - iceFigBarPlot.pdf: Figure 9 - comparison of functional connectivity to lake ice phenology
    - iceNoObsPlot.pdf: Figure 4 - number of ice observations used in ice phenology calculations
    - iceDayLength.pdf: Figure S2 - a figure comparing lake ice fraction to daylength at Nuiqsut, Alaska

## File variable descriptions for files included in the data repository
   - ColvilleDataExport_20211022.csv: Lake reflectance data - from google earth engine script no. 1
      - ID: Lake ID
      - system.time_start_mean: The time of the observation--values need to be divided by 1000 before hey can be converted to formal dates
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
   - ColvilleChannelExport_1km_20211022.csv, ColvilleChannelExport_2km_20211022.csv, ColvilleChannelExport_5km_20211022.csv, ColvilleChannelExport_10km_20211022.csv: Channel reflectance data from google earth engine script no.1. Includes average reflectance data for each lake on each date within a 1km, 2km, 5km, or 10km buffer
     	- Same as above file, except no ID or delta column, and has one additional column, described below
     	- buffer: distance around the lake to search for channel pixels in kilometers
   - ColvilleShapefilesEdited.shp: The lake polygon shapefiles
      - ID: Lake ID
      - delta: name of the delta--should always be 'colville'
      - type: 'delta'--should always be 'delta'
      - geometry: the geometry of the lake polygon
   - colvilleValidation20200508.csv: Connectivity validation data from GECI imagery
        - Row.Number: Index
        - ID: Lake ID
        - Connected: "y" channel present, "m" uncertain channel presence, "n" no channel presence
        - note: any notes made during analysis
   - colville_dt_20211022: The results of the functional connectivity classification for each lake within each time period using a decision tree-based method. This is the final classification method used in the final analysis for the paper. RData file. Can be read using read_rds() function in R. 
        - ID: Lake ID
        - time_period: The time period (string) for the classification including temporal periods (“2000-2004”, “2005-2009”, “2010-2014”, “2015-2019”), discharge periods (“1”, “2”, “3”, “4”), and the GECI validation period 2013-2016 (“validation”) 
        - .pred_class: The predicted functional connectivity of the lake within the specified time period (“connected” or “not connected”)
        - split: whether or not the lake (during the validation period) was in the training or testing group. If the observation is from outside the validation period, split = "Neither" 
   - colville_pct_20211022: The results of the functional connectivity classification for each lake within each time period using a percentage-based method. RData file. Can be read using read_rds() function in R. 
        - ID: Lake ID
        - time_period: The time period (string) for the classification including temporal periods (“2000-2004”, “2005-2009”, “2010-2014”, “2015-2019”), discharge periods (“1”, “2”, “3”, “4”), and the GECI validation period 2013-2016 (“validation”) 
        - .pred_class: The predicted functional connectivity of the lake within the specified time period (“connected” or “not connected”)
        - split: whether or not the lake (during the validation period) was in the training or testing group. If the observation is from outside the validation period, split = "Neither"
   - colville_rf_20211022: The results of the functional connectivity classification for each lake within each time period using a random forest-based method.RData file. Can be read using read_rds() function in R. 
        - ID: Lake ID
        - time_period: The time period (string) for the classification including temporal periods (“2000-2004”, “2005-2009”, “2010-2014”, “2015-2019”), discharge periods (“1”, “2”, “3”, “4”), and the GECI validation period 2013-2016 (“validation”) 
        - .pred_class: The predicted functional connectivity of the lake within the specified time period (“connected” or “not connected”)
        - split: whether or not the lake (during the validation period) was in the training or testing group. If the observation is from outside the validation period, split = "Neither"  
   - colville_km_20211022: The results of the functional connectivity classification for each lake within each time period using a kmeans-based method. RData file. Can be read using read_rds() function in R. 
        - ID: Lake ID
        - time_period: The time period (string) for the classification including temporal periods (“2000-2004”, “2005-2009”, “2010-2014”, “2015-2019”), discharge periods (“1”, “2”, “3”, “4”), and the GECI validation period 2013-2016 (“validation”) 
        - .pred_class: The predicted functional connectivity of the lake within the specified time period (“connected” or “not connected”)
        - type: describes which lake-to-channel band ratio was used for the classification
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
   - AprilLakeElev20152017.csv: Mean April lake elevation in 2015 and 2017 from ArcticDEM strip data
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
   - lake_ice_modeled_20210304.RData: Modeled daily ice fraction data from Landsat
        - ID: Lake ID
        - period: either "BU" for breakup period or "FU" for freeze-up period
        - fitted: Modeled ice fraction, ranges from 0 to 1. 
        - doy: day of the calendar year
        - ice_flag: 2 corresponds to ice fractions greater than 0.80, 1 corresponds to ice fractions between 0.20- 0.80, 0 corresponds to ice fractions less than 0.20
   - NumberOfObsTrans_20210304.RData: number of ice observations from Landsat for each lake within both the breakup and freeze-up periods 
        - ID: Lake ID
        - count_bu: number of ice observations used one week prior through one week after the modeled breakup period
        - count_fu: number of ice observations used one week prior through one week after the modeled freeze-up period
   - 150_Alaska_nolakes_widths.shp: Alaskan rivers from the GRWL Summary Statistics database (Allen & Pavelsky, 2018) that are wider than 150m. 
        - Join_Count: number of point grwl observations contained in the summary grwl reach. 
        - id: Reach ID (not the same as GRWL reach IDs)
        - nchan: number of channels in the reach
        - lakeFlag: whether or not the channel is flagged as a lake (should always be 0, which corresponds to not a lake)
        - elevm: lake elevation (m)
        - widthm: mean reach width (m)
        - width_sd: standard deviation in reach width (m)
        - width_num: same as widthm
        - geometry: reach polyline geometry
   - lakeCoverFraction_1b786b4795b34fa035d55c102cc305e7.csv: Output from step 2 of data download of raw lake ice fraction data, included in data repository
        - ID: Lake ID
        - snowIce: Like ice fraction calculated using Landsat Fmask (fraction, 0-1)
        - SLIDE_snowIce: Lake ice fraction calculated using the new SLIDE method from Yang et al. (2021) (fraction, 0-1)
        - cloud: Lake cloud fraction (fraction 0-1)
        - water: Lake water fraction (fraction, 0-1)
        - clear: Lake clear sky/land fraction (fraction, 0-1)
        - delta: Name of delta--should always be "colville"
        - CLOUD_COVER: Landsat scene total cloudcover (percent, 0-100)
        - system:time_start: time of observation (needs to be divided by 1000 to be converted to a date)
        - doy: day of year 
        - LANDSAT_SCENE_ID: Landsat scene ID
