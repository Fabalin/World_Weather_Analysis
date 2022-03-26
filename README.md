# World_Weather_Analysis
Using API's to design a travel itinerary based on defined weather parameters and performing regression analysis using SciPy.  

## Overview 
  PlanMyTripp app allows users to plan their travel itinerary based on user specified criteria. The app uses two Primary API's to accomplish this task: OpenWeatherMap and Google Maps. The API calls are centered around cities identified using the Citipy module. Based on 2,000 randomly generated latitudes and longitudes, Citipy identifies the nearest city with a population of 500 or more based on the geographic input (Source: [Citipy Git Hub](https://github.com/wingchen/citipy)). Google Maps API's search parameters were set find *lodging* within a *radius of 5,000 meters*. The data collected from the API calls per city are: 
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

The data will be combined into a dataframe and regression analysis will be performed on the weather parameters based on latitudes. The analysis will be divided into northern and southern hemispheres. Afterwards, Gmaps module's google maps with heatmap layer will be used to generate heatmap of cities based on max temperatures. An additional markers layer will be implemented to allow users to display pertinent information of each city using a pop-up marker. 

### Challenge Overview
  A customer's travel itinerary will be mapped using 4 cities, after filtering the dataframe based on maximum and minimum temperatures specified. The 4 cities are within the same country and the trip will be mapped based on driving routes. Pop-ups of each city's name, country code, hotel and weather conditions will be displayed separately. The Challenge items are separated into folders below:
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
  Code files for performing the regression anaysis and the challenge were separate and each processes has its own randomly generated set of cities. Although the data collected for each processes used the same code and were within 3 days of each other, the datasets are ultimately different in composition. Refer to [cities.csv](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/cities.csv) for regression analysis data (706 cities) and [WeatherPy_Database](https://github.com/Fabalin/World_Weather_Analysis/blob/main/Weather_Database/WeatherPy_Database.csv) for itinerary creation (692 cities). Out of the weather parameters analyzed, only Maximum Temperature provided sufficient correation with latitudes, based on R-squared values. The R-squared values for the remaining weather parameters were close to 0.01, indicating that regression analysis cannot determine a sufficient relationship to latitude. For this reason, the analysis will focus solely on the Maximum Temperature. Without additional algorithms and modules that can account for multiple weather parameters simultaneously, it is impossible to accurately predict weather patterns using regression analysis. To explore the results of the remaining parameters, refer to [WeatherPy.ipynb](https://github.com/Fabalin/World_Weather_Analysis/blob/main/WeatherPy.ipynb).  


### Regression Analysis and Heat Map of Maximum Temperature 
![City Latitude vs Max Temperature](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Fig1.png)

![Linear Regression on Northern Hemisphere](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Fig5.png)

![Linear Regression on Southern Hemisphere](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Fig6.png)

  The temperature has a dual relationship with latitude and can be observed in the first figure. The scatter plot of temperatures based on cities shows a parabolic shape with the vertex at 0 latitude. There is a plateau around -20 and 20 latitudes and a sharp decline on the right side with a steady decline on the left side. There are 492 cities in the northern hemisphere but 214 cities in the southern hemisphere translating to fewer data points to the left of 0 latitudes, suggesting a potential sample bias. This is due the northern hemisphere having more land mass (40% based on ratio between land mass and water) than the southern hemisphere (20%) (Source:[Climate Science Investigations](http://www.ces.fau.edu/nasa/module-3/regional-temperature/explanation2.php#:~:text=The%20Mollwiede%20projection%20of%20Earth,water%20in%20the%20Northern%20Hemisphere)). This effect could also be amplified by chance since cities can only exist on land masses and are identified based on randomly generated latitudes and longitudes. This disparity between sample sizes of different hemispheres also translates to the robustness of the regression model generated based on the R-squared values, as seen in the subsequent plots. The Northern Hemisphere displays a strong R-squared value of 0.79, with a negative slope indicating a negative correlation between latitude and temperature. This is not the case for the Southern Hemisphere as the R-squared value is significantly less at 0.44 and the slope is positive; which suggests that not only is the regression model less accurate in predicting changes in temperature based on latitude, but that there is also a positive correlation between temperature and latitude. These findings support the current understanding that temperatures around the equator (0 latitude) are warmer than those away from the equator and can be visually confirmed using the data generated heatmap below (Source: [BBC](https://www.bbc.co.uk/bitesize/guides/zrrjkty/revision/2#:~:text=at%20the%20poles-,Why%20is%20it%20hot%20at%20the%20Equator%20and%20cold%20at,quickly%20compared%20to%20the%20poles.)). 

![Temperature HeatMap](https://github.com/Fabalin/World_Weather_Analysis/blob/main/weather_data/Temperature_HeatMap.PNG)

### Travel Itinerary for Mauritius: An Island Getaway
  To validate the code, a sample itinerary was constructed based on temperature specifications between 70 and 95 F. Using this criteria to filter the dataframe generated in the Weather_Database folder, 267 cities were identified in the Vacation_Search.ipynb file, located in the folder with the same name. Within this code file, an API call to Google Maps was made to retrieve hotel data associated with the latitudes and longitudes of the cities. The first hotel result was integrated into the filtered dataframe and cities without hotels were eliminated. This resulted in a final total of 249 cities that contained hotels within the cities of the specified temperature range. The output data WeatherPy_vacation.csv was called in a separate code file in the folder Vacation_Itinerary to construct the itinerary. 

![Travel Destinations](https://github.com/Fabalin/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map_markers.PNG)

  **Maurititus** is a country of small islands in East Africa off the coast of Madagascar and Mozambique. According to [Google Search](https://www.google.com/search?gs_ssp=eJzj4tDP1TcwKSu2NGD04sxNLC3KLMksLQYAQs0Gyw&q=mauritius&rlz=1C1CHBF_enUS973US973&oq=ma&aqs=chrome.1.69i59j46i39j69i59l2j69i57j69i60l2j69i61.2745j0j4&sourceid=chrome&ie=UTF-8), it is a popular tourist destination with a population of 1.266 million and  
> is known for its beaches, lagoons and reefs. The mountainous interior includes Black River Gorges National Park, with rainforests, waterfalls, hiking trails and wildlife like the flying fox. Capital Port Louis has sites such as the Champs de Mars horse track, Eureka plantation house and 18th-century Sir Seewoosagur Ramgoolam Botanical Gardens.

![Trip](https://github.com/Fabalin/World_Weather_Analysis/blob/main/Vacation_Itinerary/WeatherPy_travel_map.PNG)

  The trip is centered around the eastern coast of the main island. The trip would begin at the city Souillac, bisect the island by driving to Cap Malheureux and proceeding across the eastern coast. There will be stops at Quatre Cocos and Bambous Virieux before finally returning to Souillac. It is important to note that although this report refers to these sites as cities, the google description refers to them as villages. This is because the definition of city in this report is based on that of the Citipy module. Despite missing key landmarks such as the capital Port Louis and the Chamarel Seven Colored Earth Geopark on the western coast, the trip traverses through historical sites coupled with gorgeous island scenery, such as Cap Malhereux. Cap Malhereux was named by the French colonials who failed to defend the territory from a succeessful British invasion due to having concentrated defenses on the Southern coast; a memory forever captured in the name translated as the "Unlucky Cape" (Source: [Maurititus Attractions](https://mauritiusattractions.com/cap-malheureux-i-381.html)). Clear turqouise waters accented by colourful red striped boats and yellow sandy beaches shaded by tropical trees paint the city as a vibrant yet relaxing port town. This less well known island paradise is the ideal getaway from a crowded city.      
