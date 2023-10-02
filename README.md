# WeatherPy & VacationPy API Challenge

## Introduction
In this assignment, we'll work with Matplotlib, Numpy, Linregress, and Hvplot to analyze and visualize data related to weather and vacation planning.

## Prep
 **Add .gitignore File:**
   - To protect  the API key, create a `.gitignore` file.
   - Open the `.gitignore` file and add the following line:

     ```
     # Adding config.py file.
     api_keys.py
     ```
Enter API keys for OpenWeatherMap and Geoapify in `api_keys.py` file.

## Part 1: WeatherPy

### WeatherPy

1.  **Data Retrieval:** Imports the necessary libraries and load the API keys from `api_keys.py`. Then generate a list of random latitude and longitude coordinates for the `citipy` library.  Fetch the weather data in an imperial unit for these cities from the OpenWeatherMap API and store it in a DataFrame.
```python
# Set the API base URL
base_url = "http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=" + weather_api_key
...
    # Create endpoint URL with each city
    city_url = f"{base_url}&q={city.replace(' ',  '+')}"
    ...
        # Parse out latitude, longitude, max temp, humidity, cloudiness, wind speed, country, and date
        city_lat = city_weather["coord"]["lat"]
        city_lng = city_weather["coord"]["lon"]
        city_max_temp = city_weather["main"]["temp_max"]
        city_humidity = city_weather["main"]["humidity"]
        city_clouds = city_weather["clouds"]["all"]
        city_wind = city_weather["wind"]["speed"]
        city_country = city_weather["sys"]["country"]
        city_date = city_weather["dt"]
```
    
2.  **Scatter Plot for Latitude vs. Temperature/Humidity/Cloudiness/Wind Speed:** Create a scatter plot to visualize the relationship between latitude and the four variables individually for the cities. Save the graph in `output_data`.

```python
# Build scatter plot for latitude vs. VARIABLE
plt.scatter(city_data_df["Lat"], city_data_df["VARIABLE"])
```
    
3.  **Linear Regression Prep:** Define the `plot_linear_regression` function, then calculate the linear regression info needed for the regression line from the data points. Then, customizes the appearance of the scatter plot. Define and store the cities in the Nothern vs. Southern Hemisphere based on their latitude.

```python
# Define a function to create Linear Regression plots
def plot_linear_regression(x_values, y_values, title, x_label, y_label):
    
    # Perform linear regression
    (slope, intercept, r_value, p_value, std_err) = linregress(x_values, y_values)

    # Calculate the regression line's y-values
    regression_values = x_values * slope + intercept
    
    # Plot the regression line
    plt.plot(x_values, regression_values, "r-", label=f"y = {round(slope,  2)}x + {round(intercept,  2)}")
    
    # Add the linear regression formula to the graph
    formula_text = f"y = {round(slope,  2)}x + {round(intercept,  2)}"
    plt.annotate(formula_text, (x_values.max(), y_values.max()), color="red")
```

4. **Linear Regression Plot to Compare Northern/Southern Hemisphere vs. Temperature/Humidity/Cloudiness/Wind Speed:** Create a linear regression plot to  compare the relationship between the two hemispheres and the four variables.
```python
# Select data for the Northern Hemisphere (Latitude >= 0)
northern_hemi_df = city_data_df[city_data_df["Lat"] >= 0]
...
plot_linear_regression(x_values, y_values, title, x_label, y_label)
```

### VacationPy

1.  **Data Loading:** Imports libraries and loads the CSV file created in WeatherPy into a Pandas DataFrame.
    
2.  **Creating a Map:** Use `hvplot` to display points for every city in the DataFrame. The size of each point corresponds to the humidity level in that city.
```python
# Configure the map plot
map_plot = city_data_df.hvplot.points(
    "Lng", 
    "Lat", 
    geo = True, 
    size = "Humidity",
    scale = 1,
    color = "City",
    alpha = 0.5,
    tiles = "OSM",
)
```
3.  **Filtering for Ideal Weather Conditions:**
```python
ideal_weather_df_F = city_data_df[max_temp_condition_F & humidity_condition & wind_speed_condition & cloudiness_condition].copy()
```
4.  **Creating a Hotel DataFrame and Finding Nearest Hotels:** Create a DataFrame of the hotels in the ideal weather condition cities using Geoapify API.

```python
# Set parameters to search for a hotel
radius=10000
params = {
    "categories": "accommodation", # Search for lodging places
    "filter": "circle: radius",
    "apiKey": geoapify_key,
}

# Iterate through the hotel_df DataFrame
for index, row in hotel_df.iterrows():
    # Get latitude and longitude from the DataFrame
    lat = row["Lat"]
    lng = row["Lng"]
    
    # Add filter and bias parameters with the current city's latitude and longitude to the params dictionary
    params["filter"] = f"circle:{lng},{lat},{radius}"
    params["bias"] = f"proximity:{lng},{lat}"

    # Make an API request using the params dictionary
    response = requests.get(base_url, params=params)
    
    # Convert the API response to JSON format
    name_address = response.json()
```
5.  **Mapping Hotels:** Create a map that only displays the cities with the ideal weather conditions and hotels available within the range, along with their hotel names and countries in the hover message.

## Analysis

1.  **Temperature vs. Latitude:** There is a clear linear relationship between temperature and latitude, with a decrease in temperature further away from the equator. This indicates that latitude is a primary driver of temperature.
    
2.  **Humidity vs. Latitude:** While there is a slight difference in humidity levels between the Northern and Southern Hemispheres. However, it's not significant enough to conclude that latitude is the primary driver of humidity levels.
    
3.  **Cloudiness vs. Latitude:** The relationship between cloudiness and latitude is unclear. In the Northern Hemisphere, cloudiness slightly increases with distance from the equator, while in the Southern Hemisphere, it decreases. This suggests that cloudiness is influenced by factors beyond just latitude.
    
4.  **Wind Speed vs. Latitude:** Wind speed slightly differs between the Northern and Southern Hemispheres. In the Northern Hemisphere, wind speed remains consistently low, while in the Southern Hemisphere, it increases as you move closer to the South Pole. This indicates that factors beyond latitude may influence wind speed.

5. **Data Limitation:** It should be noted that the number of data points for the Northern Hemisphere is significantly higher than that of the Southern Hemisphere. Therefore, the analysis for the Northern Hem has a higher statistical accuracy compared to the analysis of the Southern Hem. In this analysis, the temperature, humidity, cloudiness, and wind speed data are based on the cities in the two hemispheres rather than an analysis of the hemispheres themselves.

## References
[scipy.linregress](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.linregress.html)  
[Linear Regression Example](https://scikit-learn.org/stable/auto_examples/linear_model/plot_ols.html)  
[hvPlot](https://hvplot.holoviz.org/reference/geopandas/points.html)  
[Geoapify](https://apidocs.geoapify.com/docs/places/#about)  
