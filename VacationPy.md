# Dependencies Data Access 


```python
# Import the dependencies.
import pandas as pd
import numpy as np
import gmaps
import requests
# Import the API key.
from config import g_key
```


```python
# Store the CSV you saved created in part one into a DataFrame.
city_data_df = pd.read_csv("weather_data/cities.csv")
city_data_df.dtypes
```




    City_ID         int64
    City           object
    Country        object
    Date           object
    Lat           float64
    Lng           float64
    Max Temp      float64
    Humidity        int64
    Cloudiness      int64
    Wind Speed    float64
    dtype: object




```python
# Configure gmaps to use your Google API key.
gmaps.configure(api_key=g_key)
```

# Create Heatmaps 

## Temperature


```python
# Heatmap of temperature
# Get the latitude and longitude.
locations = city_data_df[["Lat", "Lng"]]
# Get the maximum temperature.
max_temp = city_data_df["Max Temp"]
# Assign the figure variable and adjust zoom, intensity and point radius. Get the middle of the earth Lat and Long: (30,31).
fig = gmaps.figure(center=(30.0, 31.0), zoom_level=1.5)
# Assign the heatmap variable.
heat_layer = gmaps.heatmap_layer(locations, weights=[max(temp, 0) for temp in max_temp], dissipating=False, max_intensity=300, point_radius=4)
# Add the heatmap layer.
fig.add_layer(heat_layer)
# Call figure to plot 
fig 
```


    Figure(layout=FigureLayout(height='420px'))


## Humidity 


```python
# Heatmap of percent humidity
locations = city_data_df[["Lat", "Lng"]]
humidity = city_data_df["Humidity"]
fig = gmaps.figure(center=(30.0, 31.0), zoom_level=1.5)
heat_layer = gmaps.heatmap_layer(locations, weights=humidity, dissipating=False, max_intensity=300, point_radius=4)

fig.add_layer(heat_layer)
# Call the figure to plot the data.
fig
```


    Figure(layout=FigureLayout(height='420px'))


## Cloudiness


```python
# Heatmap of percent Cloudiness
locations = city_data_df[["Lat", "Lng"]]
clouds = city_data_df["Cloudiness"]
fig = gmaps.figure(center=(30.0, 31.0), zoom_level=1.5)
heat_layer = gmaps.heatmap_layer(locations, weights=clouds, dissipating=False, max_intensity=300, point_radius=4)

fig.add_layer(heat_layer)
# Call the figure to plot the data.
fig
```


    Figure(layout=FigureLayout(height='420px'))


## Wind Speed


```python
# Heatmap of percent Cloudiness
locations = city_data_df[["Lat", "Lng"]]
winds = city_data_df["Wind Speed"]
fig = gmaps.figure(center=(30.0, 31.0), zoom_level=1.5)
heat_layer = gmaps.heatmap_layer(locations, weights=winds, dissipating=False, max_intensity=300, point_radius=4)

fig.add_layer(heat_layer)
# Call the figure to plot the data.
fig
```


    Figure(layout=FigureLayout(height='420px'))


# Display Information about Select Cities Based on Customer Defined Temperature 


```python
# Ask the customer to add a minimum and maximum temperature value.
min_temp = float(input("What is the minimum temperature you would like for your trip? "))
max_temp = float(input("What is the maximum temperature you would like for your trip? "))
```

    What is the minimum temperature you would like for your trip? 70
    What is the maximum temperature you would like for your trip? 95
    


```python
# Filter the dataset to find the cities that fit the criteria.
preferred_cities_df = city_data_df.loc[(city_data_df["Max Temp"] <= max_temp) & (city_data_df["Max Temp"] >= min_temp)].dropna()
preferred_cities_df.count()
```




    City_ID       284
    City          284
    Country       284
    Date          284
    Lat           284
    Lng           284
    Max Temp      284
    Humidity      284
    Cloudiness    284
    Wind Speed    284
    dtype: int64



## Look Up First Hotel near Preferred Cities


```python
# Create a Separate DF for the Hotels Data
hotel_df = preferred_cities_df[["City", "Country", "Max Temp", "Lat", "Lng"]].copy()
hotel_df["Hotel Name"] = ""
hotel_df.count()
```




    City          284
    Country       284
    Max Temp      284
    Lat           284
    Lng           284
    Hotel Name    284
    dtype: int64




```python
# Set parameters to search for a hotel.
params = {
    "radius": 5000,
    "type": "lodging",
    "key": g_key
}
```


```python
# Iterate through the DataFrame.
for index, row in hotel_df.iterrows():
    # Get the latitude and longitude.
    lat = row["Lat"]
    lng = row["Lng"]
       
    # Add the latitude and longitude to location key for the params dictionary.
    params["location"] = f"{lat},{lng}"
    # Use the search term: "lodging" and our latitude and longitude.
    base_url = "https://maps.googleapis.com/maps/api/place/nearbysearch/json?"
    # Make request and get the JSON data from the search.
    hotels = requests.get(base_url, params=params).json()
    
    # Grab the first hotel from the results and store the name.
    try:
        hotel_df.loc[index, "Hotel Name"] = hotels["results"][0]["name"]
    except (IndexError):
        print("Hotel not found... skipping.")
```

    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    Hotel not found... skipping.
    


```python
# Replace Empty Strings with No Hotels Found
hotel_df['Hotel Name'] = hotel_df['Hotel Name'].replace('', 'No Lodging within 5,000 meters')
```


```python
# Define the Info box- the desired values/ terms from the data frame. 
info_box_template = """
<dl>
<dt>Hotel Name</dt><dd>{Hotel Name}</dd>
<dt>City</dt><dd>{City}</dd>
<dt>Country</dt><dd>{Country}</dd>
<dt>Max Temp</dt><dd>{Max Temp} °F</dd>
</dl>
"""
```


```python
# Add a heatmap of temperature for the vacation spots.
locations = hotel_df[["Lat", "Lng"]]
max_temp = hotel_df["Max Temp"]
fig = gmaps.figure(center=(30.0, 31.0), zoom_level=1.5)

# Specify Layers and Marker Info 
heat_layer = gmaps.heatmap_layer(locations, weights=max_temp, dissipating=False,
             max_intensity=300, point_radius=4)


# Store the DataFrame Row.
hotel_info = [info_box_template.format(**row) for index, row in hotel_df.iterrows()]

marker_layer = gmaps.marker_layer(locations, info_box_content=hotel_info)

# Add to Figure 
fig.add_layer(heat_layer)
fig.add_layer(marker_layer)

# Call the figure to plot the data.
fig

```


    Figure(layout=FigureLayout(height='420px'))

