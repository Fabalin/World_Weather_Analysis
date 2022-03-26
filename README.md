# World_Weather_Analysis
Using API's to design a travel itinerary based on defined weather parameters. Performing regression analysis using SciPy  

## Overview 
PlanMyTripp app allows users to plan their travel itinerary based on user specified criteria. The app uses two Primary API's to accomplish this task: OpenWeatherMap and Google Maps. The API calls are centered around cities identified using the Citipy module, based on 2,000 randomly generated latitudes and longitudes. Citipy identifies the nearest city with a population of 500 or more based on the geographic input (Source: [Citipy Git Hub](https://github.com/wingchen/citipy)). Google Maps API's search parameters were set find *lodging* within a *radius of 5,000 meters*. The data collected from the API calls per city are: 
- *OpenWeatherMap*
  - City Name 
  - Country Code 
  - Date of API call
  - Latitude and Longitude  
  - Max Temperature (F) 
  - % Humidity
  - % Cloudiness 
  - Windspeed (mph)
  - Weather Description
- *Google Maps API*
  - First Identified Hotel 

The data will be combined into a dataframe and regression analysis will be performed on the weather parameters based on latitudes within the northern and southern hemispheres. Afterwards, Gmaps module's google maps with heatmap layer will be used to generate heatmap of cities based on max temperatures. An additional markers layer will be implemented to allow users to display pertinent information of each city using a pop-up marker. 

### Challenge Overview
A customer's travel itinerary will be mapped based on 4 cities after filtering the dataframe based on maximum and minimum temperatures specified. The 4 cities are within the same country and the trip will be mapped between the 4 cities based on driving routes. Pop-ups of each city's name, country code, hotel and weather conditions will be displayed separately. The Challenge items are separated into folders below:
- [Weather_Database](https://github.com/Fabalin/World_Weather_Analysis/tree/main/Weather_Database)
- [Vacation_Search](https://github.com/Fabalin/World_Weather_Analysis/tree/main/Vacation_Search)
- [Vacation_Itinerary](https://github.com/Fabalin/World_Weather_Analysis/tree/main/Vacation_Itinerary)

## Software & API 
- Anaconda - 4.11.0
- Jupyter Notebook - 6.4.8
- Python - 3.7.11
- OpenWeatherMap - [Current Weather Data](https://openweathermap.org/current)
- [Google Maps Platform](https://console.cloud.google.com/google/maps-apis/overview?project=second-casing-345123)  

### Highlight of Key Dependencies in Python
- Pandas - 1.4.1
- NumPy - 1.22.0
- SciPy - 1.8.0
- Citipy - 0.0.5
- Requests Library - 2.27.1
- Matplotlib - 3.5.1
- gmaps - 0.8.2

## Results
Code files for performing the regression anaysis and the challenge were separate and each process has its own randomly generated cities. Although the data was collected for each process were within 3 days of each other and use the same code, the datasets are ultimately different in composition. Refer to [cities.csv](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/cities.csv) for regression analysis data (706 cities) and [WeatherPy_Database](https://github.com/Fabalin/World_Weather_Analysis/blob/main/Weather_Database/WeatherPy_Database.csv) for itinerary creation (692 cities). Out of the weather parameters analyzed, Only Maximum Temperature provided sufficient correation to determine relation with latitudes, based on R-squared values. The R-squared values of the remaining weather parameters were close to 0.01, indicating that regression analysis cannot determine a sufficient relationship to latitude. For this reason, the analysis will focus solely on the Maximum Temperature. To explore the results of the remaining parameters, refer to [WeatherPy.ipynb](https://github.com/Fabalin/World_Weather_Analysis/blob/main/WeatherPy.ipynb) 


## Regression Analysis and Heat Map of Maximum Temperature 
![City Latitude vs Max Temperature](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Fig1.png)
![Linear Regression on Northern Hemisphere](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Fig5.png)
![Linear Regression on Southern Hemisphere](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Fig6.png)

The temperature has a dual relationship with latitude and can be observed in the first figure. The scatter plot of temperatures based on cities shows a parabolic shape with the vertex at 0 latitude. There is a plateau around -20 and 20 latitudes and a sharp decline on the right side with a slower decline on the left side. There are 492 cities in the northern hemisphere but 214 cities in the southern hemisphere translating to fewer data points to the left of 0 latitudes, suggesting a potential sample bias. This is due the northern hemisphere having more land mass (40% based on ratio between land mass and water) than the southern hemisphere (20%) (Source:[Climate Science Investigations](http://www.ces.fau.edu/nasa/module-3/regional-temperature/explanation2.php#:~:text=The%20Mollwiede%20projection%20of%20Earth,water%20in%20the%20Northern%20Hemisphere)). This effect could also be amplified by chance since cities can only exist on land masses and are identified based on randomly generated latitudes and longitudes. This disparity between sample sizes of different hemispheres also translates to the robustness of the regression model generated based on the R-squared values, as seen in the subsequent plots. The Northern Hemisphere displays a strong R-squared value of 0.79 with a negative slope indicating a negative correlation between the the latitude and temperature. This is not the case for the Southern Hemisphere as the R-squared value is significantly less at 0.44 and the slope is positive; which suggests that not only is the regression model less accurate in predicting changes in temperature based on latitude, but that there is also a positive correlation between temperature and latitude. These findings support the current understanding that temperatures around the equator (0 latitude) are warmer than those away from the equator and can be visually confirmed in the heatmap below (Source: [BBC](https://www.bbc.co.uk/bitesize/guides/zrrjkty/revision/2#:~:text=at%20the%20poles-,Why%20is%20it%20hot%20at%20the%20Equator%20and%20cold%20at,quickly%20compared%20to%20the%20poles.)). 

![Temperature HeatMap](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Temperature_HeatMap.PNG)
