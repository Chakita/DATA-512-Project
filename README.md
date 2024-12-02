# Analysis of Wildfire Impact on Redmond, OR

The aim of this analysis is to draw attention to the long-term and far reaching negative consequences of poorly handled wildfires. We first estimate the impact of just the smoke from nearby wildfires by devising a heuristic formula. We then attempt to evaluate the effectiveness of our smoke estimate by comparing it to AQI values. Finally we use our smoke estimate to find the impact of these wildfires on employment in the leisure and hospitality sectors of Lubbock. This is an important question to ask due to the revenue that the hospitality industry rakes in and contributes towards the economy. In 2023, visitors to and within Texas spent over $90 billion, creating an economic impact that supported 1.3 million Texas jobs and bolstered the state’s economy.

A full report can be found [here](https://github.com/Chakita/DATA-512-Project/blob/master/Reports/Final%20Report.pdf).

## Python & Jupyter Set-Up
This work assumes that users have a working Jupyter Notebook & Python 3 setup. Instructions on installing them can be found [here](https://docs.jupyter.org/en/latest/install/notebook-classic.html). It should be noted that Python modules required for this work comprise some standard modules that are installed with Python and others that are installed through the [Anaconda](https://docs.jupyter.org/en/latest/install/notebook-classic.html) distribution.

If modules are not found, they can be readily installed with the following terminal commands:

```bash
    pip install <module name>
```
or
```bash
   conda install <module name>
```

## Repository structure
```
|   LICENSE
|   README.md
|
+---data
|       smoke_employment_forecasts.csv
|       wildfire_smoke_estimate.csv
|       yearly_AQI_1961-2021.csv
|
+---extension plan
|       extension-plan-eda.ipynb
|       extension-plan-modeling.ipynb
|
+---intermediate_data
|       final_merged_yearly.csv
|
+---plots
|       average_smoke_estimate_per_year.png
|       Comparison of AQI with Smoke estimate.png
|       cross_correlation_smoke_employment.png
|       fire histogram based on distance.png
|       fires_within_650_miles.png
|       fire_size_by_year.png
|       frequency_of_types_of_wildfires.png
|       lesiure_employment_forecast.png
|       linear relationship AQI smoke estimate.png
|       non-linear relationships AQI smoke estimate.png
|       normalized_trends_smoke_esimtate_employment.png
|       time series forecast.png
|       total acres of land burned.png
|       yearly_smoke_employment_correlation.png
|
+---Reports
|       Final Report.pdf
|       Reflection.pdf
|
\---Smoke_estimate_AQI_analysis_notebooks
        AQI_data_extraction_Lubbock.ipynb
        wildfire-forecasting.ipynb

```
### Code
The code for accessing and analyzing the wildfire data can be found in the following files:
* [`wildfire-forecasting.ipynb`](https://github.com/Chakita/DATA-512-Project/blob/master/Smoke_estimate_AQI_analysis_notebooks/AQI_data_extraction_Lubbock.ipynb): Jupyter Notebook providing a detailed walkthrough of the entire data exploration, analysis, and storage process of the Wildfire data. Additionally, includes code for the smoke estimates & forecasting as well as the comparison of the estimate and the AQI data.
* [`AQI_data_extraction_Lubbock.ipynb`](https://github.com/Chakita/DATA-512-Project/blob/master/Smoke_estimate_AQI_analysis_notebooks/wildfire-forecasting.ipynb): An interactive Jupyter Notebook providing a detailed walkthrough of the entire data acquisition, analysis, and storage process of the EPA AQS AQI data.
Note that some portions of the code were developed by Dr. David McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code was provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). The rest of the code lies under the standard [MIT license](./LICENSE).

* [`extension-plan-eda.ipynb`](https://github.com/Chakita/DATA-512-Project/blob/master/extension%20plan/extension-plan-eda.ipynb): Notebook for exploratory data analysis for the Lubbock leisure and service industries
* [`extension-plan-modeling.ipynb`](https://github.com/Chakita/DATA-512-Project/blob/master/extension%20plan/extension-plan-modeling.ipynb): Notebook containing code for models developed to forecast employment and smoke estimates for Lubbock till 2045.

### Output Files
#### Data
* [`./data/wildfire_smoke_estimate.csv`](https://github.com/Chakita/DATA-512-Project/blob/master/data/wildfire_smoke_estimate.csv): Contains the final calculated smoke estimates from 1961 to 2021. The data has the following variables:
  * `name`: the name of wildfire (if assigned any)
  * `year`: the year of the fire season
  * `start date`: the start date of the wildfire
  * `end date`: the end date of the wildfire
  * `type`: the type of the wildfire
  * `distance`: the distance of the wildfire from Lubbock, Texas
  * `size`: The size of the wildfire specified by the number of GIS acres burned
  * `smoke_estimate`: The calculated smoke estimate based on the distance, size and type.

* [`./data/ yearly_AQI_1961-2021.csv`](https://github.com/Chakita/DATA-512-Project/blob/master/data/yearly_AQI_1961-2021.csv): Contains the final average max daily summary AQI during the fire season from 1961 to 2021. The data has 2 variables:
  * `year`: the year
  * `aqi`: the annual average AQI value for the fire season.
* [`./data/smoke_employment_forecasts`](https://github.com/Chakita/DATA-512-Project/blob/master/data/smoke_employment_forecasts.csv): Contains the forecasted annual smoke estimate and employment in leisure and service sectors until 2045. The data has 4 variables:
  * `year`: the year of the fire season
  * `smoke_estimate`: the average daily smoke intake during the fire season. The forecast is done through an ARIMA model.
  * `value_leisure`: The number of employees in the leisure sector in Lubbock from 1990 - 2021
  * `value_service`: the number of employees in the service sector in Lubbock from 1990 - 2021

#### Images
* [`average_smoke_estimate_per_year.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/average_smoke_estimate_per_year.png): A time series graph showing average smoke estimates near Lubbock fluctuating between 3-8 units from 1960-2020, with a notable spike around 1990.
* [`Comparison of AQI with Smoke estimate.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/Comparison%20of%20AQI%20with%20Smoke%20estimate.png): A dual-axis plot comparing smoke estimates (orange) and Air Quality Index (blue) from 1960-2020, showing varying patterns of correlation and significant AQI spikes after 2000.
* [`cross_correlation_smoke_employment.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/cross_correlation_smoke_employment.png): A cross-correlation analysis between smoke estimates and employment in leisure/service sectors, showing a consistent negative correlation of approximately -0.13 across different time lags.
* [`fire histogram based on distance.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/fire%20histogram%20based%20on%20distance.png): A histogram displaying the distribution of fire distances with 50-mile bins, peaking around 700 miles, with a red vertical line marking a 650-mile cutoff point.
* [`fires_within_650_miles.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/fires_within_650_miles.png): Number of fires occuring at a max distance of 650 miles from Lubbock
* [`frequency_of_types_of_wildfires.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/frequency_of_types_of_wildfires.png): A bar chart showing the distribution of wildfire types within 650 miles of Lubbock, with regular wildfires being the most common at nearly 10,000 occurrences.
* [`lesiure_employment_forecast.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/lesiure_employment_forecast.png): A time series plot showing historical leisure employment data from 1990-2020 with significant volatility, followed by two prediction lines extending to 2040.
* [`linear relationship AQI smoke estimate.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/linear%20relationship%20AQI%20smoke%20estimate.png): A scatter plot of residuals versus smoke estimates showing a dispersed pattern around a zero line, with residuals ranging from -10 to 40.
* [`non-linear relationships AQI smoke estimate.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/non-linear%20relationships%20AQI%20smoke%20estimate.png): A correlation analysis between smoke estimates and AQI showing weak relationships (Pearson: -0.010, Spearman: 0.058) with data points scattered across the plot.
* [`time series forecast.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/time%20series%20forecast.png): A time series forecast showing historical smoke estimates from 1960-2020 with ARIMA and exponential smoothing predictions through 2040, accompanied by model residuals analysis below.
* [`total acres of land burned.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/total%20acres%20of%20land%20burned.png): A line graph showing total acres burned within 650 miles radius from 1980-2020, displaying a dramatic increase in burned acreage over recent decades with peaks exceeding 6 million acres.
* [`normalized_trends_smoke_esimtate_employment.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/normalized_trends_smoke_esimtate_employment.png): A normalized trend comparison of smoke estimates against leisure and service employment from 1990-2020, showing steady employment growth while smoke levels remain relatively volatile and low.
* [`yearly_smoke_employment_correlation.png`](https://github.com/Chakita/DATA-512-Project/blob/master/plots/yearly_smoke_employment_correlation.png): Two scatter plots demonstrating negative correlations between smoke estimates and both leisure and service employment, with trend lines indicating declining employment as smoke estimates increase.


### Intermediate Files
* [`final_merged_yearly.csv`](https://github.com/Chakita/DATA-512-Project/blob/master/intermediate_data/final_merged_yearly.csv): Contains the final calculated smoke estimates from 1990 to 2021 along with the corresponding values of the number of employees in the leisure and hospitality sector in Lubbock. The data has the following variables:
  * `name`: the name of wildfire (if assigned any)
  * `year`: the year of the fire season
  * `start date`: the start date of the wildfire
  * `end date`: the end date of the wildfire
  * `type`: the type of the wildfire
  * `distance`: the distance of the wildfire from Lubbock, Texas
  * `smoke_estimate`: The calculated smoke estimate based on the distance, size and type.
  * `value_leisure`: The number of employees in the leisure sector in Lubbock from 1990 - 2021
  * `value_service`: the number of employees in the service sector in Lubbock from 1990 - 2021

## Data Sources

### Wildfire GeoJSON Data

The data used for this analysis was the [ USGS data for Combined wildland fire datasets for the United States and certain territories, 1800s-Present (combined wildland fire polygons) ](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81), specifically the [GeoJSON_Files.zip file](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) The data was collected and compiled by the US Geological Survey. The data includes geographic information for wildfires in the USA ranging from 1800 to 2021.
The geometry in the data are polygons that are expressed in a coordinate system with the well-known ID (WKID) 102008,which also known as [ESRI:102008](https://epsg.io/102008).
The dataset can be cited as : Welty, J.L., and Jeffries, M.I., 2021, Combined wildland fire datasets for the United States and certain territories, 1800s-Present: U.S. Geological Survey data release, https://doi.org/10.5066/P9ZXGFY3.


The GeoJSON has the following higher level keys:
* `displayFieldName`: an empty string that would otherwise denote the name of the dataset.
* `fieldAliases`: a dictionary that converts the variable name in the file to a more human-readable format for the 30 fields.
* `geometryType`: `esriGeometryPolygon` is the default geometry format.
* [`spatialReference`](https://developers.arcgis.com/web-map-specification/objects/spatialReference/): the well-known ID (WKID) of the spatial reference (as well as the latest WKID). This data uses [ESRI:102008](https://epsg.io/102008), which refers to the Albers equal-area map projection of North America.
* `fields`: A list of dictionaries identifying the `name`, `type`, and `alias` of 30 attributes. The `alias` is the same as those in `fieldAliases`. Also, the `types` are based on the [Esri specifications](https://developers.arcgis.com/web-map-specification/objects/field/). A full, detailed explanation of each of the attributes can be found [here](./input/Wildland_Fire_Polygon_Metadata.xml).
* `features`: the list of all observations stored in JSON format. Note that each observation is saved as a dictionary with keys for the `attributes` (same as those in `fields`) and `geometry`, which contains sets of tuples denoting the coordinate points of the wildfire polygon in the WKID projection space. In the case of this data, most wildfires are polygons represented by ['rings'](https://developers.arcgis.com/documentation/common-data-types/geometry-objects.htm#:~:text=%7B%0A%20%20%22paths%22%3A%20%5B%20%5D%0A%7D-,Polygon,-A%20polygon%20(specified)) in ArcGIS. A ring is a list of points denoting the path of the ring. The exterior ring of a polygon is denoted by a list of points oriented clockwise while internal rings are points oriented in a counterclockwise motion. The `rings` key for `geometry` has multiple polygons decreasing in area. The first 'ring' denotes the largest boundary of the fire. Subsequent rings follow an even-odd fill rule (first fills, 2nd removes, etc). A few wildfires are represented by [`curveRings`](https://developers.arcgis.com/documentation/common-data-types/geometry-objects.htm#:~:text=y%5D%2C%20%5Bx%2C%20y%5D%5D%7D-,Polyline%20with%20curves,-A%20polyline%20with), which are similar to `rings` except that they define certain portions of a ring using parameters for preset curve functions rather than individual data points.

where 'features' represents one wildfire in the data set and includes (but not limited to) the following information:
- Fire type ranging from Wildfire,Likely Wildfire, Unknown-Likely Wildfire, Unknown- Likely Prescribed Fire, Prescibed Fire
- Fire year
- Assigned Fire Names (if any)
- GIS Acres and Hectares burned
- rings: A list of polygon rings defining the overall fire polygon, with the first one denoting the fire perimeter.
- curveRings: A list of polygon curveRings defining the overall shape, with the first one denoting the fire perimeter

Every feature has a 'geometry' which specifies geo coordinates that represent a geographic object ans in this case the wildfires are bounded shapes, circles, squares and so on represented by the 'rings' field in GeoJSON. The largest shape (ring) is supposed to be item zero in the list of 'rings'.
One caveat of performing calcualtions on the geo coordinates directly is that since they represent points on the earth and the earth is not flat,the computation of distance between two points is more of an arc as opposed to a straight line and varies depending on the position of the two points. To mitigate this issue, we use the PyProj module to accurately calculate the straight line distance between our geographic coordinates.

To calculate the distance of a fire from a given city we have two options:
1) To calculate distance based on the perimeter of the fire, using the point on the fire perimeter closest to the city as the starting point
2) To calculate the average distance of all points on the fire perimeter to the given city - this is roughly the same as calculating the distance from the centeroid.

Since the fires could have irreguarly shaped boundaries, we choose to go with the second apporach of measuring distance from roughly the center of the fire to the city.

#### Filtering the data

We filter the data based on the following critieria
1. Year range between 1961 and 2021.
2. Distance for the city is at most 1800 miles. Note that in the smoke estimate, we only consider fires that are 650 miles away from the city but for visualization purposes, we include a broader radius of 1800 miles
3. Fires occuring during fire season of a given year i.e between May and Oct. If we are unable to parse a date including month for a given fire, we assume that it was in the fire season and proceed.

After filtering, we store the desired subset into [`data/wildfire_smoke_estimate.csv`](https://github.com/Chakita/DATA-512-Project/blob/master/data/wildfire_smoke_estimate.csv).


### Air Quality Index data
We use the Air Quality index as a metric to compare and sanity test our smoke estimate. We get the AQI data from  US EPA Air Quality System (AQS) API. We calcualte yearly smoke estimates and yearly AQI for Lubbock,Texas and check for correlation between the two. The data provided by this API is historic and not real-time air quality data. The [documentation](https://aqs.epa.gov/aqsweb/documents/data_api.html)
for the API provides definitions of the different call parameter and examples of the various calls that can be made to the API. A thorough explanation of how AQI is calculated can be found [here](https://www.airnow.gov/sites/default/files/2020-05/aqi-technical-assistance-document-sept2018.pdf). The terms of use can be found [here](https://aqs.epa.gov/aqsweb/documents/data_api.html#terms). All data accessed through the API lies in the [public domain](https://edg.epa.gov/epa_data_license.html).
Specifically, we used the maximal daily average sensor data for monitoring stations in Deschutes County, all of which were <17 miles from Redmond, OR. These daily max values were then averaged for the fire season to get the annual estimate for 1983-2023. There was no data available before 1983. Finding nearby monitoring stations requires the Federal Information Processing Series (FIPS) of the desired city, county, and state. Information was gathered from [here](https://www.census.gov/library/reference/code-lists/ansi.html#cou). A detailed walkthrough of the data collection process can be found in [`./analysis_part1-AQI.ipynb`](./analysis_part1-AQI.ipynb). The final data can be found as [`./output/final_annual_AQI_1983-2023.csv`](./output/final_annual_AQI_1983-2023.csv).



### Lubbock Leisure and Service Jobs Data
For my analysis focus areas, I shortlisted the following data sourced from the [U.S Bureau of Labor Statistics](https://data.bls.gov/dataQuery/find?st=7420&r=20&s=popularity%3AD&q=lubbock&more=0) website. The data available is highly granular and localized to Lubbock and can be easily downloaded as a CSV file from their website. I picked the datasets since they are open to use by the public (with citation of the Bureau of Labor Statistics as the source), they are closely related to the questions of interest in our analysis and the data is at the city level which reduces the processing overhead of extracting rows of interest from a larger dataset.

1. **Labor Participation Rate: [https://data.bls.gov/dataViewer/view](https://data.bls.gov/dataViewer/view)**
   This data is gathered as part of the State and Metro Area Employment, Hours, & Earnings estimates for non-farm workers. The data is the rate of labor participation (expressed as a percentage) for the years 1961 \- 2021\. It includes information across all industries and occupations. Demographic are civilian individuals from all races, genders, education levels and ages 16 and above.
2. **Employed and Office of Employment and Unemployment Statistics : Service-Providing : [https://data.bls.gov/dataViewer/view/timeseries/SMU48311800700000001](https://data.bls.gov/dataViewer/view/timeseries/SMU48311800700000001)**
   This data is also gathered as part of the State and Metro Area Employment, Hours, & Earnings estimates for non-farm workers. The data contains information regarding the number of employees (in thousands) in the service industry in Lubbock, Texas in the years 1961 to 2021\.  It includes information across all industries and occupations. Demographic are civilian individuals from all races, genders, education levels and ages 16 and above.

3. **Employed and Office of Employment and Unemployment Statistics : Leisure and Hospitality :**
   [**https://data.bls.gov/dataViewer/view/timeseries/SMU48311807000000001**](https://data.bls.gov/dataViewer/view/timeseries/SMU48311807000000001)
   This data is also gathered as part of the State and Metro Area Employment, Hours, & Earnings estimates for non-farm workers. The data contains information regarding the number of employees (in thousands) in the hospitality industry in Lubbock, Texas in the years 1961 to 2021\. It includes information across all industries and occupations. Demographic are civilian individuals from all races, genders, education levels and ages 16 and above.

According to the Bureau of Labor Statistics, the data is public domain and free to use with proper citation to the source.

### Limitations
The current review, though extensive, operates within certain critical parameters and limitations that need to be recognized in the interpretation of the results:

#### Data Distributions
We make assumptions about the properties of the data distribution such as normality assumptions where appropriate and also that the data is independent to the best of our knowledge. However, these assumptions make it so that our analysis is only a rough guideline rather than a precise prediction/forecast. Extensive survey and statistical analysis is required to get a more precise idea.

#### Economic Data Complexity
However, the employment statistics in both the leisure and service industries are influenced by a much more complex set of elements than just the environmental factors. More major economic events, especially recessions, have shown massive impacts on the course of employment trends. For example, the financial crisis of 2008 and the COVID-19 pandemic of 2020 caused unprecedented disruptions in these sectors. Finally, seasonal changes, regional economic regulatory shifts, and broader market dynamics all impact employment disparities, rendering it very difficult to isolate the exact impact of wildfire smoke.

#### Environmental Confounding Variables
The diversified climate of Texas harbors different environmental challenges that may impact economic activities. In fact, from 1980 to 2024, the state has faced 19 major drought and heat wave events, accounting for more than 50% of weather-related deaths in Texas. Such extreme weather patterns can significantly alter outdoor activities and attractions and may pose confounding variables within our analysis of the impacts of wildfire smoke. Other meteorological events, such as thunderstorms and tornadoes, also have similar effects on the economic activities of leisure and services sectors, further complicating the effort to attribute variations solely to exposure to wildfire smoke.

#### Smoke Estimate Model Limitations
Our smoke estimate model, while providing a structured approach to quantifying wildfire impact, has several limitations.
Wind Dynamics: The current model does not consider the speed and direction of winds in the dynamical system governing smoke dispersion. Stronger winds will therefore move the smoke to farther distances or in completely erratic directions, which can lead to under- and overestimates of smoke exposure in defined regions.
Atmospheric Conditions: The model does not account for variables such as humidity, temperature inversions, and atmospheric stability—all of which can have a strong influence on smoke behavior and concentration.


These caveats mean that the smoke estimates presented should be taken as approximate indicators rather than precise quantifications of smoke exposure. Additionally, the model provides a basic framework that can be refined in future work by incorporating additional meteorological and atmospheric variables. Grasping these limitations is important for the proper interpretation of our findings and points to possible ways future research might be conducted.



## Notes
### Smoke Estimation

To calculate the smoke estimate, we consider the following fields: GIS acres burned (Wildfire size), distance from the Wildfire to Lubbock (calculated as described above) and the type of wildfire.
The impact of a wildfire is most likely influenced directly by how big the wildfire was - the GIS acres burned and the type of wildfire capture this. The impact of wildfire is also likely to be less severe if it occurs at a greater distance from Lubbock and vice-versa. So we consider the smoke estimate to be directly proportional to wildfire size and inversely proportional to distance from city and also influenced to varying degrees based on wildfire type. To capture this, we come up with the following relationship between smoke estimate and the fields considered: $$Smoke\_estimate = \alpha * (\frac{1}{distance}) * \beta * size + \gamma $$ where $\alpha$ and $\beta$ are tunable weightage given to distance of wildfire from city and size of wildfire respectively. These account for the proportinality constant as well. $\gamma$ is an additonal weight added upon based on the ordinal mapping between the type of wildfire and the proabable severity. The ordinal mapping is as follows:
```
 {
    'Wildfire': 4,
    'Likely Wildfire': 3,
    'Unknown - Likely Wildfire': 2,
    'Unknown - Likely Prescribed Fire': 1,
    'Prescribed Fire': 0
    }
 ```
 The value of $\alpha$ and $\beta$ are set to 0.5 so as to give distance and wildfire size equal weightage while computing smoke estimate.

 ## Conclusion

This study provides results showing a significant relationship between wildfire smoke exposure and employment levels in the leisure and service sectors of Lubbock. The strong negative correlations found in the annual data suggest that the overall effects of wildfire smoke exposure may have significant economic impacts on the city.

It should be noted that correlation is not the same as causation. While our findings show an association between smoke estimates and employment levels, there may be other factors not studied in this paper that could help explain these trends. Future research should include more variables, such as broader economic conditions, policy changes, and other environmental factors, to better understand the relationship between wildfires and local economic conditions.

To improve this analysis, the following changes are suggested:
- Integrate wind data alongside advanced atmospheric modeling techniques to enhance the precision of smoke estimations.
- Perform a comparative analysis across multiple cities to ascertain whether the identified trends are unique to Lubbock or indicative of a more extensive regional pattern.
- Do an in-depth analysis of specific subsectors of leisure and service industries to establish the ones most vulnerable to the impacts of wildfires.
- Integrate qualitative information from local businesses and residents to provide contextual meaning to the quantitative findings.
- Broaden the analysis to include more recent data (after 2021) in order to grasp the impacts of recent large wildfire events.

In summary, this study, while very important for the potential economic impacts of wildfires in Lubbock, should be taken as a starting point for broader research and policy discussions. The findings have emphatically brought out the need for proactivity in taking steps to address both environmental and economic challenges brought about by the increased risk of wildfires in the region
