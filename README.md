# Wildfire Analysis for Lubbock,Texas

This repositoruy contains the code and data for estimating impact of smoke caused by wildfires on the city of Lubbock, Texas. the aim of this project is to try to come up with an estimate to determine the impact of smoke from the wildfires around the city of Lubbock,Texas and try to come up with a prediction of what this estimate looks like for the next 25 years. Parts of the code used for processing the JSON file contianing geographic information were adapted from [example code](https://drive.google.com/file/d/1B7AGlaW7d-27bHKLVXGBwLt8T-Elx-HB/view?usp=drive_link) provided by Dr.David McDonald. 
Further the regular expression code to process start and end dates for fires was shared by [Himanshu J. Naidu](https://github.com/himanshunaidu/data512_project/blob/master/wildfire_data_acquisition.ipynb). 

## Repository structure
The repository contains the code for the data processing and analysis on smoke estimate in the [wildfire-forecasting.ipynb notebook](https://github.com/Chakita/DATA-512-Project/blob/master/wildfire-forecasting.ipynb) and contains the code for AQI data collection and processing in the [AQI_data_extraction_Lubbock.ipynb notebook](https://github.com/Chakita/DATA-512-Project/blob/master/AQI_data_extraction_Lubbock.ipynb).
The processed and cleaned data can be found in the [data folder](https://github.com/Chakita/DATA-512-Project/tree/master/data). The plots generated during the analysis can be found in the [plots folder](https://github.com/Chakita/DATA-512-Project/tree/master/plots)

## Data

### Wildfire GeoJSON data

The data used for this analysis was the [ USGS data for Combined wildland fire datasets for the United States and certain territories, 1800s-Present (combined wildland fire polygons) ](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81), specifically the [GeoJSON_Files.zip file](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) The data was collected and compiled by the US Geological Survey. The data includes geographic information for wildfires in the USA ranging from 1800 to 2021.
The geometry in the data are polygons that are expressed in a coordinate system with the well-known ID (WKID) 102008,which also known as [ESRI:102008](https://epsg.io/102008).

The GeoJSON has the following higher level keys:
- 'displayFieldName'
- 'fieldAliases'
- 'geometryType'
- 'spatialReference'
- 'fields'
- 'features'
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

### AQI data
We use the Air Quality index as a metric to compare and sanity test our smoke estimate. We get the AQI data from  US EPA Air Quality System (AQS) API. We calcualte yearly smoke estimates and yearly AQI for Lubbock and check for correlation between the two. The data provided by this API is historic and not real-time air quality data. The [documentation](https://aqs.epa.gov/aqsweb/documents/data_api.html)
for the API provides definitions of the different call parameter and examples of the various calls that can be made to the API.


# Estimating the impact of smoke
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

# Correlation with AQI data

We calculated Pearson correlation coefficient between yearly smoke estimates and AQI and we find a weak neagtive correlation. The Pearson correlation coefficient makes a number of assumptions about the underlying distribution such as normality assumptions. Our data doesn't meet these assumptions so we cannot rely on the Pearson coefficient. To check for non-linear realtionships,we check Spearman and Kendall coefficients which also only indicate weak correlation. Spearman and Kendall correlation coefficients are non-parametric in nature and do not rely on assumptions about the underlying data distribution.
- Pearson Correlation: -0.010494941020912517
- Spearman Correlation: 0.05846784222938628
- Kendall Correlation: 0.047845767357425836

From further analysis checking for non-linear relationships, we could not find any and those we assume that there is no significant relationship between our smoke estimates and AQI.

## Forecasting smoke estimates for next 25 years

We use timeseries models namely the exponential smooting model and ARIMA to forecast the some estimate for the next 25 years. These models are best suited for yearly data without any seasonality in them. Since our smoke estimates are yearly data, we employ these models for forecasting. Based on the MSE, MAE and MAPE accuracy metrics, we see that exponential smoothing performs slightly better than ARIMA

# Conclusiomn
We estimated the impact of smoke produced by wildfires on the city of Lubbock. We compare our calcualted metric with AQI (air quality index) in order to try to figure out if our estimate is good. Our analysis shows no significant relationship between our calculated smoke impact and AQI values. Upon further inspection into how AQI values are calculated, it might not be the best yardstick to evaluate our metric that measures wildfire impact. The AQI (Air Quality Index) is not an ideal metric for specifically measuring wildfire smoke impact, due to its calculation methodology. AQI takes the highest individual pollutant reading among all measured pollutants (like PM2.5, PM10, O3, CO, SO2, NO2) and uses that single value to determine the overall AQI score. While wildfire smoke primarily impacts PM2.5 (fine particulate matter) levels, other pollutants that aren't directly related to wildfire smoke could be driving the AQI value on any given day. For example, a city could have a high AQI due to elevated ozone levels from vehicle emissions, even with minimal wildfire smoke present. Conversely, a moderate PM2.5 increase from wildfire smoke might be masked in the AQI if another pollutant is reading higher. A more accurate assessment of wildfire smoke impact would focus specifically on PM2.5 concentrations or use specialized smoke estimation models that account for fire activity, meteorology, and smoke transport patterns.

