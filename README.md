
# Python API Challenge
## Part 1: WeatherPy
### Setting Up the Project:
 - Create a new repository on GitHub and clone it to your local computer.
 - Organize your project folders as instructed (create a folder for WeatherPy).
 - Add the necessary files: api_keys.py, WeatherPy.ipynb, and .gitignore.
```python
git init
git clone [repository url]
mkdir WeatherPy VacationPy
touch .gitignore
vim .gitignore
# Adding config.py file.
api_keys.py
git status
git add
git commit
git push`
pip install citipy #if not installed
```
### Retrieve City Data:
- Use the citipy library to generate random coordinates and find the nearest city for each coordinate. Then store the city data in a DataFrame.
### Retrieve Weather Data:
- Use the OpenWeatherMap API to fetch weather data for each city in your DataFrame. Then store the weather data in the DataFrame.
### Create Scatter Plots:
- Using matplotlib or a similar library, create scatter plots to visualize the relationships between latitude and weather variables (temperature, humidity, cloudiness, wind speed).
### Perform Linear Regression:
- For each scatter plot, separate the data into Northern and Southern Hemispheres based on latitude.
- Calculate linear regression for each hemisphere's data.
- Create scatter plots with regression lines and display the regression equation and r-value on the plots.  
## Part 2: VacationPy
### Retrieve City Data and Filter:
- Load the CSV file containing city data created in WeatherPy.
- Filter the DataFrame to find cities with your ideal weather conditions (e.g., temperature between 21°C and 27°C, low wind speed, zero cloudiness).
### Create a Map:
- Use the geoViews Python library to create a map that displays a point for each city in the filtered DataFrame.
- Size the points based on humidity.
### Find Nearby Hotels:
- Create a new DataFrame (hotel_df) to store information about hotels.
- For each city in your filtered DataFrame, use the Geoapify API to find the first hotel located within 10,000 meters of the city's coordinates.
### Add Hotel Information to Map:
- Modify your map to include information about the hotel name and country as a hover message for each city's point on the map.
