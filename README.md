# Weather API
HW-wk 6

The objective of this assignment was to analyze the effect of latitude on the following weather variables: temperature, humidity, cloudiness, and wind speed. To get the 500 randomly selected cities I used numpy to randomly generate values for latitude and longitude then zipped the values together and saved to a list and matched to the nearest city using the Citypy Python library. I then pulled the requested weather data Using those city locations using the Open Weather API. I used this data to generate scatter plots using Matplotlib to analyze the correlation between latitude and other weather variables. 

key findings can be found at the end.

```python
# Dependencies and Setup
import matplotlib.pyplot as plt
%matplotlib inline
import pandas as pd
import numpy as np
import requests
import time
from matplotlib import cm
import datetime

# Import API key
from config import *

# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# Output File (CSV)
output_data_file = "output_data/cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```

## Generate Cities List


```python
# List for holding lat_lngs and cities
lat_lngs=[]
cities = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)


# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    
    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)

# Print the city count to confirm sufficient count
len(cities)

```




    591



## Perform API Calls

Temperature (F)  |  Humidity (%)   |   Cloudiness (%)   |  Wind Speed (mph) 


```python
# Starting URL for Weather Map API Call
# url = "http://api.openweathermap.org/data/2.5/weather?q={}{}&units=Imperial".format(city, owm_key).replace(" ", "%20")
temp = []
humidity = []
clouds = []
wind = []
lat = []
lng = []
valid_cities = []
city_count = 1

for city in cities:
    city_url = "http://api.openweathermap.org/data/2.5/weather?q={}{}&units=Imperial".format(city, owm_key).replace(" ", "%20")
    print(city_url)
    city_count += 1

        

#print log as cities are called
    print("Retrieving Results for city {}: {}".format(city_count, city))
    try:    
        city_weather = requests.get(city_url).json()
        temp.append(city_weather['main']['temp'])
        humidity.append(city_weather['main']['humidity'])
        clouds.append(city_weather['clouds']['all'])
        wind.append(city_weather['wind']['speed'])
        lat.append(city_weather['coord']['lat'])
        lng.append(city_weather['coord']['lon'])
        valid_cities.append(city)

    except (KeyError, IndexError):
        print("Missing result... skipping.")

```

    http://api.openweathermap.org/data/2.5/weather?q=tuggurt&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 1: tuggurt
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=arica&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 1: arica
    http://api.openweathermap.org/data/2.5/weather?q=leningradskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 2: leningradskiy
    http://api.openweathermap.org/data/2.5/weather?q=rikitea&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 3: rikitea
    http://api.openweathermap.org/data/2.5/weather?q=tsihombe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 4: tsihombe
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=vaini&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 4: vaini
    http://api.openweathermap.org/data/2.5/weather?q=namibe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 5: namibe
    http://api.openweathermap.org/data/2.5/weather?q=galle&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 6: galle
    http://api.openweathermap.org/data/2.5/weather?q=mehamn&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 7: mehamn
    http://api.openweathermap.org/data/2.5/weather?q=chuy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 8: chuy
    http://api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 9: illoqqortoormiut
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=rodrigues%20alves&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 9: rodrigues alves
    http://api.openweathermap.org/data/2.5/weather?q=carnarvon&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 10: carnarvon
    http://api.openweathermap.org/data/2.5/weather?q=albany&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 11: albany
    http://api.openweathermap.org/data/2.5/weather?q=burkhala&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 12: burkhala
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=hermanus&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 12: hermanus
    http://api.openweathermap.org/data/2.5/weather?q=chokurdakh&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 13: chokurdakh
    http://api.openweathermap.org/data/2.5/weather?q=faanui&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 14: faanui
    http://api.openweathermap.org/data/2.5/weather?q=punta%20arenas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 15: punta arenas
    http://api.openweathermap.org/data/2.5/weather?q=hamilton&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 16: hamilton
    http://api.openweathermap.org/data/2.5/weather?q=mys%20shmidta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 17: mys shmidta
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=vaitupu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 17: vaitupu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=rio%20gallegos&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 17: rio gallegos
    http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 18: tasiilaq
    http://api.openweathermap.org/data/2.5/weather?q=qaanaaq&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 19: qaanaaq
    http://api.openweathermap.org/data/2.5/weather?q=tuktoyaktuk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 20: tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?q=kapaa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 21: kapaa
    http://api.openweathermap.org/data/2.5/weather?q=tsovagyugh&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 22: tsovagyugh
    http://api.openweathermap.org/data/2.5/weather?q=linxia&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 23: linxia
    http://api.openweathermap.org/data/2.5/weather?q=manali&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 24: manali
    http://api.openweathermap.org/data/2.5/weather?q=belushya%20guba&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 25: belushya guba
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=atuona&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 25: atuona
    http://api.openweathermap.org/data/2.5/weather?q=sentyabrskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 26: sentyabrskiy
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=kaitangata&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 26: kaitangata
    http://api.openweathermap.org/data/2.5/weather?q=ushuaia&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 27: ushuaia
    http://api.openweathermap.org/data/2.5/weather?q=pochutla&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 28: pochutla
    http://api.openweathermap.org/data/2.5/weather?q=hofn&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 29: hofn
    http://api.openweathermap.org/data/2.5/weather?q=el%20alto&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 30: el alto
    http://api.openweathermap.org/data/2.5/weather?q=mar%20del%20plata&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 31: mar del plata
    http://api.openweathermap.org/data/2.5/weather?q=fortuna&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 32: fortuna
    http://api.openweathermap.org/data/2.5/weather?q=bluff&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 33: bluff
    http://api.openweathermap.org/data/2.5/weather?q=barrow&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 34: barrow
    http://api.openweathermap.org/data/2.5/weather?q=faya&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 35: faya
    http://api.openweathermap.org/data/2.5/weather?q=khatanga&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 36: khatanga
    http://api.openweathermap.org/data/2.5/weather?q=georgetown&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 37: georgetown
    http://api.openweathermap.org/data/2.5/weather?q=anadyr&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 38: anadyr
    http://api.openweathermap.org/data/2.5/weather?q=dali&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 39: dali
    http://api.openweathermap.org/data/2.5/weather?q=castro&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 40: castro
    http://api.openweathermap.org/data/2.5/weather?q=fort%20nelson&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 41: fort nelson
    http://api.openweathermap.org/data/2.5/weather?q=lac%20du%20bonnet&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 42: lac du bonnet
    http://api.openweathermap.org/data/2.5/weather?q=saint%20anthony&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 43: saint anthony
    http://api.openweathermap.org/data/2.5/weather?q=xianshuigu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 44: xianshuigu
    http://api.openweathermap.org/data/2.5/weather?q=constitucion&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 45: constitucion
    http://api.openweathermap.org/data/2.5/weather?q=young&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 46: young
    http://api.openweathermap.org/data/2.5/weather?q=bonavista&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 47: bonavista
    http://api.openweathermap.org/data/2.5/weather?q=cangucu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 48: cangucu
    http://api.openweathermap.org/data/2.5/weather?q=saskylakh&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 49: saskylakh
    http://api.openweathermap.org/data/2.5/weather?q=cherskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 50: cherskiy
    http://api.openweathermap.org/data/2.5/weather?q=coquimbo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 51: coquimbo
    http://api.openweathermap.org/data/2.5/weather?q=alta%20floresta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 52: alta floresta
    http://api.openweathermap.org/data/2.5/weather?q=mahebourg&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 53: mahebourg
    http://api.openweathermap.org/data/2.5/weather?q=yeletskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 54: yeletskiy
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=ilulissat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 54: ilulissat
    http://api.openweathermap.org/data/2.5/weather?q=floresta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 55: floresta
    http://api.openweathermap.org/data/2.5/weather?q=taolanaro&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 56: taolanaro
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=ahipara&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 56: ahipara
    http://api.openweathermap.org/data/2.5/weather?q=mataura&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 57: mataura
    http://api.openweathermap.org/data/2.5/weather?q=hobart&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 58: hobart
    http://api.openweathermap.org/data/2.5/weather?q=guerrero%20negro&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 59: guerrero negro
    http://api.openweathermap.org/data/2.5/weather?q=new%20norfolk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 60: new norfolk
    http://api.openweathermap.org/data/2.5/weather?q=caravelas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 61: caravelas
    http://api.openweathermap.org/data/2.5/weather?q=talnakh&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 62: talnakh
    http://api.openweathermap.org/data/2.5/weather?q=hilo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 63: hilo
    http://api.openweathermap.org/data/2.5/weather?q=iberia&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 64: iberia
    http://api.openweathermap.org/data/2.5/weather?q=sur&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 65: sur
    http://api.openweathermap.org/data/2.5/weather?q=the%20valley&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 66: the valley
    http://api.openweathermap.org/data/2.5/weather?q=vardo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 67: vardo
    http://api.openweathermap.org/data/2.5/weather?q=batagay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 68: batagay
    http://api.openweathermap.org/data/2.5/weather?q=avarua&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 69: avarua
    http://api.openweathermap.org/data/2.5/weather?q=victoria&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 70: victoria
    http://api.openweathermap.org/data/2.5/weather?q=busselton&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 71: busselton
    http://api.openweathermap.org/data/2.5/weather?q=camacha&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 72: camacha
    http://api.openweathermap.org/data/2.5/weather?q=lebu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 73: lebu
    http://api.openweathermap.org/data/2.5/weather?q=hithadhoo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 74: hithadhoo
    http://api.openweathermap.org/data/2.5/weather?q=leh&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 75: leh
    http://api.openweathermap.org/data/2.5/weather?q=kavieng&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 76: kavieng
    http://api.openweathermap.org/data/2.5/weather?q=maragogi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 77: maragogi
    http://api.openweathermap.org/data/2.5/weather?q=nizhneyansk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 78: nizhneyansk
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=camopi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 78: camopi
    http://api.openweathermap.org/data/2.5/weather?q=manzhouli&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 79: manzhouli
    http://api.openweathermap.org/data/2.5/weather?q=upernavik&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 80: upernavik
    http://api.openweathermap.org/data/2.5/weather?q=zhigansk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 81: zhigansk
    http://api.openweathermap.org/data/2.5/weather?q=dejen&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 82: dejen
    http://api.openweathermap.org/data/2.5/weather?q=samarai&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 83: samarai
    http://api.openweathermap.org/data/2.5/weather?q=grand%20river%20south%20east&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 84: grand river south east
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=nome&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 84: nome
    http://api.openweathermap.org/data/2.5/weather?q=port%20elizabeth&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 85: port elizabeth
    http://api.openweathermap.org/data/2.5/weather?q=ipixuna&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 86: ipixuna
    http://api.openweathermap.org/data/2.5/weather?q=puerto%20ayora&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 87: puerto ayora
    http://api.openweathermap.org/data/2.5/weather?q=hay%20river&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 88: hay river
    http://api.openweathermap.org/data/2.5/weather?q=raikot&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 89: raikot
    http://api.openweathermap.org/data/2.5/weather?q=barentsburg&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 90: barentsburg
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=rio%20grande&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 90: rio grande
    http://api.openweathermap.org/data/2.5/weather?q=butaritari&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 91: butaritari
    http://api.openweathermap.org/data/2.5/weather?q=east%20london&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 92: east london
    http://api.openweathermap.org/data/2.5/weather?q=tiksi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 93: tiksi
    http://api.openweathermap.org/data/2.5/weather?q=bredasdorp&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 94: bredasdorp
    http://api.openweathermap.org/data/2.5/weather?q=tubuala&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 95: tubuala
    http://api.openweathermap.org/data/2.5/weather?q=nemuro&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 96: nemuro
    http://api.openweathermap.org/data/2.5/weather?q=lima&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 97: lima
    http://api.openweathermap.org/data/2.5/weather?q=bethel&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 98: bethel
    http://api.openweathermap.org/data/2.5/weather?q=ugoofaaru&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 99: ugoofaaru
    http://api.openweathermap.org/data/2.5/weather?q=esperance&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 100: esperance
    http://api.openweathermap.org/data/2.5/weather?q=jamestown&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 101: jamestown
    http://api.openweathermap.org/data/2.5/weather?q=raga&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 102: raga
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=krasnogorsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 102: krasnogorsk
    http://api.openweathermap.org/data/2.5/weather?q=gizo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 103: gizo
    http://api.openweathermap.org/data/2.5/weather?q=kristiansund&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 104: kristiansund
    http://api.openweathermap.org/data/2.5/weather?q=yulara&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 105: yulara
    http://api.openweathermap.org/data/2.5/weather?q=khvatovka&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 106: khvatovka
    http://api.openweathermap.org/data/2.5/weather?q=berestechko&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 107: berestechko
    http://api.openweathermap.org/data/2.5/weather?q=miri&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 108: miri
    http://api.openweathermap.org/data/2.5/weather?q=port%20alfred&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 109: port alfred
    http://api.openweathermap.org/data/2.5/weather?q=amderma&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 110: amderma
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=vostok&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 110: vostok
    http://api.openweathermap.org/data/2.5/weather?q=trat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 111: trat
    http://api.openweathermap.org/data/2.5/weather?q=maraa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 112: maraa
    http://api.openweathermap.org/data/2.5/weather?q=wulanhaote&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 113: wulanhaote
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=nizwa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 113: nizwa
    http://api.openweathermap.org/data/2.5/weather?q=tambovka&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 114: tambovka
    http://api.openweathermap.org/data/2.5/weather?q=narsaq&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 115: narsaq
    http://api.openweathermap.org/data/2.5/weather?q=kautokeino&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 116: kautokeino
    http://api.openweathermap.org/data/2.5/weather?q=sarkand&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 117: sarkand
    http://api.openweathermap.org/data/2.5/weather?q=cape%20town&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 118: cape town
    http://api.openweathermap.org/data/2.5/weather?q=beringovskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 119: beringovskiy
    http://api.openweathermap.org/data/2.5/weather?q=bengkulu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 120: bengkulu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=vila%20velha&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 120: vila velha
    http://api.openweathermap.org/data/2.5/weather?q=yellowknife&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 121: yellowknife
    http://api.openweathermap.org/data/2.5/weather?q=geraldton&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 122: geraldton
    http://api.openweathermap.org/data/2.5/weather?q=vidim&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 123: vidim
    http://api.openweathermap.org/data/2.5/weather?q=harlingen&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 124: harlingen
    http://api.openweathermap.org/data/2.5/weather?q=zuwarah&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 125: zuwarah
    http://api.openweathermap.org/data/2.5/weather?q=haines%20junction&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 126: haines junction
    http://api.openweathermap.org/data/2.5/weather?q=samusu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 127: samusu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=port%20lincoln&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 127: port lincoln
    http://api.openweathermap.org/data/2.5/weather?q=makung&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 128: makung
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=mamallapuram&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 128: mamallapuram
    http://api.openweathermap.org/data/2.5/weather?q=ughelli&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 129: ughelli
    http://api.openweathermap.org/data/2.5/weather?q=san%20rafael%20del%20sur&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 130: san rafael del sur
    http://api.openweathermap.org/data/2.5/weather?q=porosozero&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 131: porosozero
    http://api.openweathermap.org/data/2.5/weather?q=datong&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 132: datong
    http://api.openweathermap.org/data/2.5/weather?q=sao%20felix%20do%20xingu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 133: sao felix do xingu
    http://api.openweathermap.org/data/2.5/weather?q=rantepao&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 134: rantepao
    http://api.openweathermap.org/data/2.5/weather?q=alice%20springs&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 135: alice springs
    http://api.openweathermap.org/data/2.5/weather?q=goba&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 136: goba
    http://api.openweathermap.org/data/2.5/weather?q=lompoc&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 137: lompoc
    http://api.openweathermap.org/data/2.5/weather?q=belaya%20gora&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 138: belaya gora
    http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 139: saint-philippe
    http://api.openweathermap.org/data/2.5/weather?q=nanortalik&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 140: nanortalik
    http://api.openweathermap.org/data/2.5/weather?q=rungata&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 141: rungata
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=moses%20lake&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 141: moses lake
    http://api.openweathermap.org/data/2.5/weather?q=bafq&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 142: bafq
    http://api.openweathermap.org/data/2.5/weather?q=alofi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 143: alofi
    http://api.openweathermap.org/data/2.5/weather?q=shadegan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 144: shadegan
    http://api.openweathermap.org/data/2.5/weather?q=airai&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 145: airai
    http://api.openweathermap.org/data/2.5/weather?q=mayo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 146: mayo
    http://api.openweathermap.org/data/2.5/weather?q=aviles&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 147: aviles
    http://api.openweathermap.org/data/2.5/weather?q=severobaykalsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 148: severobaykalsk
    http://api.openweathermap.org/data/2.5/weather?q=ruatoria&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 149: ruatoria
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=wagar&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 149: wagar
    http://api.openweathermap.org/data/2.5/weather?q=asfi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 150: asfi
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=ocampo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 150: ocampo
    http://api.openweathermap.org/data/2.5/weather?q=daxian&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 151: daxian
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=khani&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 151: khani
    http://api.openweathermap.org/data/2.5/weather?q=tuatapere&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 152: tuatapere
    http://api.openweathermap.org/data/2.5/weather?q=colwyn%20bay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 153: colwyn bay
    http://api.openweathermap.org/data/2.5/weather?q=rosarito&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 154: rosarito
    http://api.openweathermap.org/data/2.5/weather?q=hambantota&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 155: hambantota
    http://api.openweathermap.org/data/2.5/weather?q=turangi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 156: turangi
    http://api.openweathermap.org/data/2.5/weather?q=attawapiskat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 157: attawapiskat
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=chulman&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 157: chulman
    http://api.openweathermap.org/data/2.5/weather?q=pato%20branco&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 158: pato branco
    http://api.openweathermap.org/data/2.5/weather?q=kirakira&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 159: kirakira
    http://api.openweathermap.org/data/2.5/weather?q=havelock&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 160: havelock
    http://api.openweathermap.org/data/2.5/weather?q=la%20ronge&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 161: la ronge
    http://api.openweathermap.org/data/2.5/weather?q=mullaitivu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 162: mullaitivu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=sistranda&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 162: sistranda
    http://api.openweathermap.org/data/2.5/weather?q=pevek&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 163: pevek
    http://api.openweathermap.org/data/2.5/weather?q=tazovskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 164: tazovskiy
    http://api.openweathermap.org/data/2.5/weather?q=malacampa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 165: malacampa
    http://api.openweathermap.org/data/2.5/weather?q=tumannyy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 166: tumannyy
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=fallon&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 166: fallon
    http://api.openweathermap.org/data/2.5/weather?q=pamplona&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 167: pamplona
    http://api.openweathermap.org/data/2.5/weather?q=kavaratti&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 168: kavaratti
    http://api.openweathermap.org/data/2.5/weather?q=the%20pas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 169: the pas
    http://api.openweathermap.org/data/2.5/weather?q=codrington&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 170: codrington
    http://api.openweathermap.org/data/2.5/weather?q=tarudant&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 171: tarudant
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=broome&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 171: broome
    http://api.openweathermap.org/data/2.5/weather?q=ichinohe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 172: ichinohe
    http://api.openweathermap.org/data/2.5/weather?q=arraial%20do%20cabo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 173: arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?q=thompson&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 174: thompson
    http://api.openweathermap.org/data/2.5/weather?q=khvorostyanka&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 175: khvorostyanka
    http://api.openweathermap.org/data/2.5/weather?q=souillac&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 176: souillac
    http://api.openweathermap.org/data/2.5/weather?q=celica&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 177: celica
    http://api.openweathermap.org/data/2.5/weather?q=rab&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 178: rab
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=huadian&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 178: huadian
    http://api.openweathermap.org/data/2.5/weather?q=oskemen&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 179: oskemen
    http://api.openweathermap.org/data/2.5/weather?q=dikson&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 180: dikson
    http://api.openweathermap.org/data/2.5/weather?q=moron&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 181: moron
    http://api.openweathermap.org/data/2.5/weather?q=coahuayana&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 182: coahuayana
    http://api.openweathermap.org/data/2.5/weather?q=mujiayingzi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 183: mujiayingzi
    http://api.openweathermap.org/data/2.5/weather?q=cidreira&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 184: cidreira
    http://api.openweathermap.org/data/2.5/weather?q=santa%20helena&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 185: santa helena
    http://api.openweathermap.org/data/2.5/weather?q=abalak&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 186: abalak
    http://api.openweathermap.org/data/2.5/weather?q=sechura&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 187: sechura
    http://api.openweathermap.org/data/2.5/weather?q=sambava&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 188: sambava
    http://api.openweathermap.org/data/2.5/weather?q=moorhead&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 189: moorhead
    http://api.openweathermap.org/data/2.5/weather?q=cabo%20san%20lucas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 190: cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?q=iracoubo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 191: iracoubo
    http://api.openweathermap.org/data/2.5/weather?q=college&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 192: college
    http://api.openweathermap.org/data/2.5/weather?q=jega&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 193: jega
    http://api.openweathermap.org/data/2.5/weather?q=ust-nera&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 194: ust-nera
    http://api.openweathermap.org/data/2.5/weather?q=sao%20filipe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 195: sao filipe
    http://api.openweathermap.org/data/2.5/weather?q=mana&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 196: mana
    http://api.openweathermap.org/data/2.5/weather?q=dvinskoy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 197: dvinskoy
    http://api.openweathermap.org/data/2.5/weather?q=san%20rafael&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 198: san rafael
    http://api.openweathermap.org/data/2.5/weather?q=jyvaskyla&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 199: jyvaskyla
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=kodinsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 199: kodinsk
    http://api.openweathermap.org/data/2.5/weather?q=severo-kurilsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 200: severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?q=chilliwack&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 201: chilliwack
    http://api.openweathermap.org/data/2.5/weather?q=huarmey&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 202: huarmey
    http://api.openweathermap.org/data/2.5/weather?q=thinadhoo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 203: thinadhoo
    http://api.openweathermap.org/data/2.5/weather?q=saint-gaudens&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 204: saint-gaudens
    http://api.openweathermap.org/data/2.5/weather?q=lorengau&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 205: lorengau
    http://api.openweathermap.org/data/2.5/weather?q=limon&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 206: limon
    http://api.openweathermap.org/data/2.5/weather?q=ereymentau&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 207: ereymentau
    http://api.openweathermap.org/data/2.5/weather?q=novyy%20urengoy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 208: novyy urengoy
    http://api.openweathermap.org/data/2.5/weather?q=pasighat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 209: pasighat
    http://api.openweathermap.org/data/2.5/weather?q=cururupu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 210: cururupu
    http://api.openweathermap.org/data/2.5/weather?q=san%20bartolo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 211: san bartolo
    http://api.openweathermap.org/data/2.5/weather?q=nishihara&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 212: nishihara
    http://api.openweathermap.org/data/2.5/weather?q=falun&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 213: falun
    http://api.openweathermap.org/data/2.5/weather?q=vysokogornyy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 214: vysokogornyy
    http://api.openweathermap.org/data/2.5/weather?q=melekhovskaya&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 215: melekhovskaya
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=lavrentiya&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 215: lavrentiya
    http://api.openweathermap.org/data/2.5/weather?q=okhotsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 216: okhotsk
    http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 217: nikolskoye
    http://api.openweathermap.org/data/2.5/weather?q=lloydminster&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 218: lloydminster
    http://api.openweathermap.org/data/2.5/weather?q=umzimvubu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 219: umzimvubu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=karauzyak&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 219: karauzyak
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=saint-andre-avellin&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 219: saint-andre-avellin
    http://api.openweathermap.org/data/2.5/weather?q=whitehorse&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 220: whitehorse
    http://api.openweathermap.org/data/2.5/weather?q=lillooet&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 221: lillooet
    http://api.openweathermap.org/data/2.5/weather?q=rocha&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 222: rocha
    http://api.openweathermap.org/data/2.5/weather?q=saldanha&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 223: saldanha
    http://api.openweathermap.org/data/2.5/weather?q=marcona&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 224: marcona
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=chapais&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 224: chapais
    http://api.openweathermap.org/data/2.5/weather?q=southbridge&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 225: southbridge
    http://api.openweathermap.org/data/2.5/weather?q=waddan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 226: waddan
    http://api.openweathermap.org/data/2.5/weather?q=coihaique&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 227: coihaique
    http://api.openweathermap.org/data/2.5/weather?q=katsuura&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 228: katsuura
    http://api.openweathermap.org/data/2.5/weather?q=cristian&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 229: cristian
    http://api.openweathermap.org/data/2.5/weather?q=richards%20bay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 230: richards bay
    http://api.openweathermap.org/data/2.5/weather?q=husavik&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 231: husavik
    http://api.openweathermap.org/data/2.5/weather?q=petrolina&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 232: petrolina
    http://api.openweathermap.org/data/2.5/weather?q=kupang&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 233: kupang
    http://api.openweathermap.org/data/2.5/weather?q=yichang&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 234: yichang
    http://api.openweathermap.org/data/2.5/weather?q=minot&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 235: minot
    http://api.openweathermap.org/data/2.5/weather?q=shache&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 236: shache
    http://api.openweathermap.org/data/2.5/weather?q=galesong&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 237: galesong
    http://api.openweathermap.org/data/2.5/weather?q=elat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 238: elat
    http://api.openweathermap.org/data/2.5/weather?q=sterna&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 239: sterna
    http://api.openweathermap.org/data/2.5/weather?q=walvis%20bay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 240: walvis bay
    http://api.openweathermap.org/data/2.5/weather?q=iqaluit&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 241: iqaluit
    http://api.openweathermap.org/data/2.5/weather?q=sept-iles&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 242: sept-iles
    http://api.openweathermap.org/data/2.5/weather?q=mont-joli&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 243: mont-joli
    http://api.openweathermap.org/data/2.5/weather?q=kamaishi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 244: kamaishi
    http://api.openweathermap.org/data/2.5/weather?q=aflu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 245: aflu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=fairbanks&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 245: fairbanks
    http://api.openweathermap.org/data/2.5/weather?q=loandjili&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 246: loandjili
    http://api.openweathermap.org/data/2.5/weather?q=klaksvik&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 247: klaksvik
    http://api.openweathermap.org/data/2.5/weather?q=puerto%20suarez&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 248: puerto suarez
    http://api.openweathermap.org/data/2.5/weather?q=sitka&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 249: sitka
    http://api.openweathermap.org/data/2.5/weather?q=ribeira%20grande&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 250: ribeira grande
    http://api.openweathermap.org/data/2.5/weather?q=yumen&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 251: yumen
    http://api.openweathermap.org/data/2.5/weather?q=charcas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 252: charcas
    http://api.openweathermap.org/data/2.5/weather?q=waingapu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 253: waingapu
    http://api.openweathermap.org/data/2.5/weather?q=bambous%20virieux&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 254: bambous virieux
    http://api.openweathermap.org/data/2.5/weather?q=acapulco&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 255: acapulco
    http://api.openweathermap.org/data/2.5/weather?q=banjar&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 256: banjar
    http://api.openweathermap.org/data/2.5/weather?q=olafsvik&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 257: olafsvik
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=bellmead&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 257: bellmead
    http://api.openweathermap.org/data/2.5/weather?q=lagunas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 258: lagunas
    http://api.openweathermap.org/data/2.5/weather?q=kudamatsu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 259: kudamatsu
    http://api.openweathermap.org/data/2.5/weather?q=satitoa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 260: satitoa
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=saurimo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 260: saurimo
    http://api.openweathermap.org/data/2.5/weather?q=labutta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 261: labutta
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=geresk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 261: geresk
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=takoradi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 261: takoradi
    http://api.openweathermap.org/data/2.5/weather?q=kodiak&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 262: kodiak
    http://api.openweathermap.org/data/2.5/weather?q=panjwin&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 263: panjwin
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=stornoway&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 263: stornoway
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=kemijarvi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 263: kemijarvi
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=srednekolymsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 263: srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?q=brandon&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 264: brandon
    http://api.openweathermap.org/data/2.5/weather?q=beian&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 265: beian
    http://api.openweathermap.org/data/2.5/weather?q=san%20juan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 266: san juan
    http://api.openweathermap.org/data/2.5/weather?q=portland&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 267: portland
    http://api.openweathermap.org/data/2.5/weather?q=ribera&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 268: ribera
    http://api.openweathermap.org/data/2.5/weather?q=san%20quintin&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 269: san quintin
    http://api.openweathermap.org/data/2.5/weather?q=hudesti&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 270: hudesti
    http://api.openweathermap.org/data/2.5/weather?q=sisimiut&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 271: sisimiut
    http://api.openweathermap.org/data/2.5/weather?q=yenotayevka&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 272: yenotayevka
    http://api.openweathermap.org/data/2.5/weather?q=buraydah&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 273: buraydah
    http://api.openweathermap.org/data/2.5/weather?q=qiongshan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 274: qiongshan
    http://api.openweathermap.org/data/2.5/weather?q=mazamari&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 275: mazamari
    http://api.openweathermap.org/data/2.5/weather?q=varhaug&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 276: varhaug
    http://api.openweathermap.org/data/2.5/weather?q=amberley&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 277: amberley
    http://api.openweathermap.org/data/2.5/weather?q=kununurra&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 278: kununurra
    http://api.openweathermap.org/data/2.5/weather?q=vestmanna&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 279: vestmanna
    http://api.openweathermap.org/data/2.5/weather?q=cap%20malheureux&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 280: cap malheureux
    http://api.openweathermap.org/data/2.5/weather?q=tocopilla&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 281: tocopilla
    http://api.openweathermap.org/data/2.5/weather?q=yomitan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 282: yomitan
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=lovozero&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 282: lovozero
    http://api.openweathermap.org/data/2.5/weather?q=hornepayne&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 283: hornepayne
    http://api.openweathermap.org/data/2.5/weather?q=saint-pierre&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 284: saint-pierre
    http://api.openweathermap.org/data/2.5/weather?q=lugovskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 285: lugovskiy
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=finnsnes&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 285: finnsnes
    http://api.openweathermap.org/data/2.5/weather?q=sao%20joao%20da%20barra&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 286: sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?q=stabat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 287: stabat
    http://api.openweathermap.org/data/2.5/weather?q=torbay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 288: torbay
    http://api.openweathermap.org/data/2.5/weather?q=saint%20george&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 289: saint george
    http://api.openweathermap.org/data/2.5/weather?q=grand%20centre&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 290: grand centre
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=hasaki&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 290: hasaki
    http://api.openweathermap.org/data/2.5/weather?q=te%20anau&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 291: te anau
    http://api.openweathermap.org/data/2.5/weather?q=prince%20rupert&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 292: prince rupert
    http://api.openweathermap.org/data/2.5/weather?q=daru&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 293: daru
    http://api.openweathermap.org/data/2.5/weather?q=nabire&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 294: nabire
    http://api.openweathermap.org/data/2.5/weather?q=sassandra&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 295: sassandra
    http://api.openweathermap.org/data/2.5/weather?q=isangel&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 296: isangel
    http://api.openweathermap.org/data/2.5/weather?q=canutama&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 297: canutama
    http://api.openweathermap.org/data/2.5/weather?q=altea&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 298: altea
    http://api.openweathermap.org/data/2.5/weather?q=port%20augusta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 299: port augusta
    http://api.openweathermap.org/data/2.5/weather?q=kholodnyy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 300: kholodnyy
    http://api.openweathermap.org/data/2.5/weather?q=cabedelo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 301: cabedelo
    http://api.openweathermap.org/data/2.5/weather?q=katherine&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 302: katherine
    http://api.openweathermap.org/data/2.5/weather?q=kenai&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 303: kenai
    http://api.openweathermap.org/data/2.5/weather?q=xining&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 304: xining
    http://api.openweathermap.org/data/2.5/weather?q=ponta%20do%20sol&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 305: ponta do sol
    http://api.openweathermap.org/data/2.5/weather?q=luanda&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 306: luanda
    http://api.openweathermap.org/data/2.5/weather?q=vanimo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 307: vanimo
    http://api.openweathermap.org/data/2.5/weather?q=ayan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 308: ayan
    http://api.openweathermap.org/data/2.5/weather?q=luderitz&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 309: luderitz
    http://api.openweathermap.org/data/2.5/weather?q=aklavik&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 310: aklavik
    http://api.openweathermap.org/data/2.5/weather?q=aykhal&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 311: aykhal
    http://api.openweathermap.org/data/2.5/weather?q=esmeraldas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 312: esmeraldas
    http://api.openweathermap.org/data/2.5/weather?q=lagoa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 313: lagoa
    http://api.openweathermap.org/data/2.5/weather?q=angoche&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 314: angoche
    http://api.openweathermap.org/data/2.5/weather?q=flin%20flon&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 315: flin flon
    http://api.openweathermap.org/data/2.5/weather?q=karratha&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 316: karratha
    http://api.openweathermap.org/data/2.5/weather?q=domoni&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 317: domoni
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=biak&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 317: biak
    http://api.openweathermap.org/data/2.5/weather?q=trelew&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 318: trelew
    http://api.openweathermap.org/data/2.5/weather?q=alto%20vera&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 319: alto vera
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=altamont&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 319: altamont
    http://api.openweathermap.org/data/2.5/weather?q=kazalinsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 320: kazalinsk
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=sompeta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 320: sompeta
    http://api.openweathermap.org/data/2.5/weather?q=salalah&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 321: salalah
    http://api.openweathermap.org/data/2.5/weather?q=kurilsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 322: kurilsk
    http://api.openweathermap.org/data/2.5/weather?q=provideniya&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 323: provideniya
    http://api.openweathermap.org/data/2.5/weather?q=savonlinna&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 324: savonlinna
    http://api.openweathermap.org/data/2.5/weather?q=mataram&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 325: mataram
    http://api.openweathermap.org/data/2.5/weather?q=sorvag&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 326: sorvag
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=maceio&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 326: maceio
    http://api.openweathermap.org/data/2.5/weather?q=erenhot&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 327: erenhot
    http://api.openweathermap.org/data/2.5/weather?q=saint-georges&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 328: saint-georges
    http://api.openweathermap.org/data/2.5/weather?q=hobyo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 329: hobyo
    http://api.openweathermap.org/data/2.5/weather?q=sarakhs&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 330: sarakhs
    http://api.openweathermap.org/data/2.5/weather?q=deputatskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 331: deputatskiy
    http://api.openweathermap.org/data/2.5/weather?q=sulangan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 332: sulangan
    http://api.openweathermap.org/data/2.5/weather?q=axim&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 333: axim
    http://api.openweathermap.org/data/2.5/weather?q=zaraza&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 334: zaraza
    http://api.openweathermap.org/data/2.5/weather?q=beloha&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 335: beloha
    http://api.openweathermap.org/data/2.5/weather?q=thongwa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 336: thongwa
    http://api.openweathermap.org/data/2.5/weather?q=norman%20wells&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 337: norman wells
    http://api.openweathermap.org/data/2.5/weather?q=aswan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 338: aswan
    http://api.openweathermap.org/data/2.5/weather?q=osoyoos&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 339: osoyoos
    http://api.openweathermap.org/data/2.5/weather?q=guaymas&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 340: guaymas
    http://api.openweathermap.org/data/2.5/weather?q=aleksandrov%20gay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 341: aleksandrov gay
    http://api.openweathermap.org/data/2.5/weather?q=vila%20franca%20do%20campo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 342: vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?q=fort%20saint%20john&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 343: fort saint john
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=berlevag&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 343: berlevag
    http://api.openweathermap.org/data/2.5/weather?q=bahia%20blanca&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 344: bahia blanca
    http://api.openweathermap.org/data/2.5/weather?q=copiapo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 345: copiapo
    http://api.openweathermap.org/data/2.5/weather?q=manggar&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 346: manggar
    http://api.openweathermap.org/data/2.5/weather?q=mweka&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 347: mweka
    http://api.openweathermap.org/data/2.5/weather?q=sembe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 348: sembe
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=gambat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 348: gambat
    http://api.openweathermap.org/data/2.5/weather?q=chunoyar&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 349: chunoyar
    http://api.openweathermap.org/data/2.5/weather?q=bandarbeyla&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 350: bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?q=yar-sale&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 351: yar-sale
    http://api.openweathermap.org/data/2.5/weather?q=ciudad%20dario&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 352: ciudad dario
    http://api.openweathermap.org/data/2.5/weather?q=petropavlovsk-kamchatskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 353: petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?q=bambanglipuro&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 354: bambanglipuro
    http://api.openweathermap.org/data/2.5/weather?q=bathsheba&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 355: bathsheba
    http://api.openweathermap.org/data/2.5/weather?q=juneau&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 356: juneau
    http://api.openweathermap.org/data/2.5/weather?q=marzuq&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 357: marzuq
    http://api.openweathermap.org/data/2.5/weather?q=narasannapeta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 358: narasannapeta
    http://api.openweathermap.org/data/2.5/weather?q=sinnamary&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 359: sinnamary
    http://api.openweathermap.org/data/2.5/weather?q=cascais&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 360: cascais
    http://api.openweathermap.org/data/2.5/weather?q=beyneu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 361: beyneu
    http://api.openweathermap.org/data/2.5/weather?q=killybegs&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 362: killybegs
    http://api.openweathermap.org/data/2.5/weather?q=tongliao&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 363: tongliao
    http://api.openweathermap.org/data/2.5/weather?q=manzil%20tamim&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 364: manzil tamim
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=verkhnevilyuysk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 364: verkhnevilyuysk
    http://api.openweathermap.org/data/2.5/weather?q=obluche&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 365: obluche
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=boa%20vista&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 365: boa vista
    http://api.openweathermap.org/data/2.5/weather?q=cairns&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 366: cairns
    http://api.openweathermap.org/data/2.5/weather?q=severnyy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 367: severnyy
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=soyo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 367: soyo
    http://api.openweathermap.org/data/2.5/weather?q=quilpue&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 368: quilpue
    http://api.openweathermap.org/data/2.5/weather?q=brookings&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 369: brookings
    http://api.openweathermap.org/data/2.5/weather?q=pisco&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 370: pisco
    http://api.openweathermap.org/data/2.5/weather?q=chernihiv&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 371: chernihiv
    http://api.openweathermap.org/data/2.5/weather?q=merauke&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 372: merauke
    http://api.openweathermap.org/data/2.5/weather?q=dossor&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 373: dossor
    http://api.openweathermap.org/data/2.5/weather?q=burica&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 374: burica
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=pirgos&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 374: pirgos
    http://api.openweathermap.org/data/2.5/weather?q=kudahuvadhoo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 375: kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?q=taltal&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 376: taltal
    http://api.openweathermap.org/data/2.5/weather?q=kieta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 377: kieta
    http://api.openweathermap.org/data/2.5/weather?q=micheweni&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 378: micheweni
    http://api.openweathermap.org/data/2.5/weather?q=selcuk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 379: selcuk
    http://api.openweathermap.org/data/2.5/weather?q=rapid%20valley&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 380: rapid valley
    http://api.openweathermap.org/data/2.5/weather?q=sydney&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 381: sydney
    http://api.openweathermap.org/data/2.5/weather?q=velikodvorskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 382: velikodvorskiy
    http://api.openweathermap.org/data/2.5/weather?q=laguna&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 383: laguna
    http://api.openweathermap.org/data/2.5/weather?q=mormugao&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 384: mormugao
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=atar&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 384: atar
    http://api.openweathermap.org/data/2.5/weather?q=tasbuget&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 385: tasbuget
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=yaan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 385: yaan
    http://api.openweathermap.org/data/2.5/weather?q=orotukan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 386: orotukan
    http://api.openweathermap.org/data/2.5/weather?q=viedma&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 387: viedma
    http://api.openweathermap.org/data/2.5/weather?q=bacolod&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 388: bacolod
    http://api.openweathermap.org/data/2.5/weather?q=puquio&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 389: puquio
    http://api.openweathermap.org/data/2.5/weather?q=ambon&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 390: ambon
    http://api.openweathermap.org/data/2.5/weather?q=orgun&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 391: orgun
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=wangaratta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 391: wangaratta
    http://api.openweathermap.org/data/2.5/weather?q=katunino&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 392: katunino
    http://api.openweathermap.org/data/2.5/weather?q=jerusalem&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 393: jerusalem
    http://api.openweathermap.org/data/2.5/weather?q=hami&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 394: hami
    http://api.openweathermap.org/data/2.5/weather?q=kalispell&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 395: kalispell
    http://api.openweathermap.org/data/2.5/weather?q=port-gentil&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 396: port-gentil
    http://api.openweathermap.org/data/2.5/weather?q=magadan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 397: magadan
    http://api.openweathermap.org/data/2.5/weather?q=galiwinku&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 398: galiwinku
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=ancud&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 398: ancud
    http://api.openweathermap.org/data/2.5/weather?q=formosa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 399: formosa
    http://api.openweathermap.org/data/2.5/weather?q=cayenne&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 400: cayenne
    http://api.openweathermap.org/data/2.5/weather?q=vao&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 401: vao
    http://api.openweathermap.org/data/2.5/weather?q=jales&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 402: jales
    http://api.openweathermap.org/data/2.5/weather?q=bima&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 403: bima
    http://api.openweathermap.org/data/2.5/weather?q=pacific%20grove&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 404: pacific grove
    http://api.openweathermap.org/data/2.5/weather?q=nouadhibou&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 405: nouadhibou
    http://api.openweathermap.org/data/2.5/weather?q=mount%20gambier&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 406: mount gambier
    http://api.openweathermap.org/data/2.5/weather?q=carutapera&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 407: carutapera
    http://api.openweathermap.org/data/2.5/weather?q=asyut&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 408: asyut
    http://api.openweathermap.org/data/2.5/weather?q=khonuu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 409: khonuu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=west%20fargo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 409: west fargo
    http://api.openweathermap.org/data/2.5/weather?q=karema&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 410: karema
    http://api.openweathermap.org/data/2.5/weather?q=abu%20kamal&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 411: abu kamal
    http://api.openweathermap.org/data/2.5/weather?q=palabuhanratu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 412: palabuhanratu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=pinheiro%20machado&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 412: pinheiro machado
    http://api.openweathermap.org/data/2.5/weather?q=jiddah&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 413: jiddah
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=nuuk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 413: nuuk
    http://api.openweathermap.org/data/2.5/weather?q=moree&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 414: moree
    http://api.openweathermap.org/data/2.5/weather?q=gladstone&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 415: gladstone
    http://api.openweathermap.org/data/2.5/weather?q=port%20macquarie&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 416: port macquarie
    http://api.openweathermap.org/data/2.5/weather?q=meulaboh&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 417: meulaboh
    http://api.openweathermap.org/data/2.5/weather?q=gijon&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 418: gijon
    http://api.openweathermap.org/data/2.5/weather?q=san%20carlos%20de%20bariloche&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 419: san carlos de bariloche
    http://api.openweathermap.org/data/2.5/weather?q=marsa%20matruh&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 420: marsa matruh
    http://api.openweathermap.org/data/2.5/weather?q=anaconda&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 421: anaconda
    http://api.openweathermap.org/data/2.5/weather?q=san%20policarpo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 422: san policarpo
    http://api.openweathermap.org/data/2.5/weather?q=harper&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 423: harper
    http://api.openweathermap.org/data/2.5/weather?q=kirkenaer&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 424: kirkenaer
    http://api.openweathermap.org/data/2.5/weather?q=hillerod&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 425: hillerod
    http://api.openweathermap.org/data/2.5/weather?q=gat&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 426: gat
    http://api.openweathermap.org/data/2.5/weather?q=san%20cristobal&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 427: san cristobal
    http://api.openweathermap.org/data/2.5/weather?q=gallup&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 428: gallup
    http://api.openweathermap.org/data/2.5/weather?q=bestobe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 429: bestobe
    http://api.openweathermap.org/data/2.5/weather?q=grimari&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 430: grimari
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=poum&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 430: poum
    http://api.openweathermap.org/data/2.5/weather?q=praia%20da%20vitoria&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 431: praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?q=palana&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 432: palana
    http://api.openweathermap.org/data/2.5/weather?q=pacifica&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 433: pacifica
    http://api.openweathermap.org/data/2.5/weather?q=elizabeth%20city&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 434: elizabeth city
    http://api.openweathermap.org/data/2.5/weather?q=maldonado&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 435: maldonado
    http://api.openweathermap.org/data/2.5/weather?q=lashio&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 436: lashio
    http://api.openweathermap.org/data/2.5/weather?q=tongzi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 437: tongzi
    http://api.openweathermap.org/data/2.5/weather?q=jumla&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 438: jumla
    http://api.openweathermap.org/data/2.5/weather?q=sohag&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 439: sohag
    http://api.openweathermap.org/data/2.5/weather?q=praxedis%20guerrero&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 440: praxedis guerrero
    http://api.openweathermap.org/data/2.5/weather?q=kutum&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 441: kutum
    http://api.openweathermap.org/data/2.5/weather?q=lata&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 442: lata
    http://api.openweathermap.org/data/2.5/weather?q=taiyuan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 443: taiyuan
    http://api.openweathermap.org/data/2.5/weather?q=kruisfontein&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 444: kruisfontein
    http://api.openweathermap.org/data/2.5/weather?q=mahibadhoo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 445: mahibadhoo
    http://api.openweathermap.org/data/2.5/weather?q=maniitsoq&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 446: maniitsoq
    http://api.openweathermap.org/data/2.5/weather?q=krasnoarmeysk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 447: krasnoarmeysk
    http://api.openweathermap.org/data/2.5/weather?q=tanglad&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 448: tanglad
    http://api.openweathermap.org/data/2.5/weather?q=valparaiso&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 449: valparaiso
    http://api.openweathermap.org/data/2.5/weather?q=bhalki&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 450: bhalki
    http://api.openweathermap.org/data/2.5/weather?q=bayshore%20gardens&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 451: bayshore gardens
    http://api.openweathermap.org/data/2.5/weather?q=logansport&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 452: logansport
    http://api.openweathermap.org/data/2.5/weather?q=rawson&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 453: rawson
    http://api.openweathermap.org/data/2.5/weather?q=ialibu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 454: ialibu
    http://api.openweathermap.org/data/2.5/weather?q=tres%20arroyos&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 455: tres arroyos
    http://api.openweathermap.org/data/2.5/weather?q=benalla&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 456: benalla
    http://api.openweathermap.org/data/2.5/weather?q=atasu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 457: atasu
    http://api.openweathermap.org/data/2.5/weather?q=kjollefjord&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 458: kjollefjord
    http://api.openweathermap.org/data/2.5/weather?q=paucartambo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 459: paucartambo
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=nchelenge&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 459: nchelenge
    http://api.openweathermap.org/data/2.5/weather?q=nicoya&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 460: nicoya
    http://api.openweathermap.org/data/2.5/weather?q=surgut&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 461: surgut
    http://api.openweathermap.org/data/2.5/weather?q=umm%20kaddadah&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 462: umm kaddadah
    http://api.openweathermap.org/data/2.5/weather?q=palmerston&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 463: palmerston
    http://api.openweathermap.org/data/2.5/weather?q=turukhansk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 464: turukhansk
    http://api.openweathermap.org/data/2.5/weather?q=malakal&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 465: malakal
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=pingliang&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 465: pingliang
    http://api.openweathermap.org/data/2.5/weather?q=charters%20towers&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 466: charters towers
    http://api.openweathermap.org/data/2.5/weather?q=sangar&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 467: sangar
    http://api.openweathermap.org/data/2.5/weather?q=kahului&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 468: kahului
    http://api.openweathermap.org/data/2.5/weather?q=jasper&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 469: jasper
    http://api.openweathermap.org/data/2.5/weather?q=muli&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 470: muli
    http://api.openweathermap.org/data/2.5/weather?q=notse&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 471: notse
    http://api.openweathermap.org/data/2.5/weather?q=verkhoyansk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 472: verkhoyansk
    http://api.openweathermap.org/data/2.5/weather?q=cervo&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 473: cervo
    http://api.openweathermap.org/data/2.5/weather?q=hualmay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 474: hualmay
    http://api.openweathermap.org/data/2.5/weather?q=san%20pedro&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 475: san pedro
    http://api.openweathermap.org/data/2.5/weather?q=namatanai&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 476: namatanai
    http://api.openweathermap.org/data/2.5/weather?q=graaff-reinet&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 477: graaff-reinet
    http://api.openweathermap.org/data/2.5/weather?q=roma&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 478: roma
    http://api.openweathermap.org/data/2.5/weather?q=burnie&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 479: burnie
    http://api.openweathermap.org/data/2.5/weather?q=willowmore&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 480: willowmore
    http://api.openweathermap.org/data/2.5/weather?q=luwuk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 481: luwuk
    http://api.openweathermap.org/data/2.5/weather?q=maralal&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 482: maralal
    http://api.openweathermap.org/data/2.5/weather?q=xai-xai&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 483: xai-xai
    http://api.openweathermap.org/data/2.5/weather?q=bonthe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 484: bonthe
    http://api.openweathermap.org/data/2.5/weather?q=cortez&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 485: cortez
    http://api.openweathermap.org/data/2.5/weather?q=ostrovnoy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 486: ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?q=les%20herbiers&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 487: les herbiers
    http://api.openweathermap.org/data/2.5/weather?q=birin&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 488: birin
    http://api.openweathermap.org/data/2.5/weather?q=gravdal&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 489: gravdal
    http://api.openweathermap.org/data/2.5/weather?q=rio%20branco&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 490: rio branco
    http://api.openweathermap.org/data/2.5/weather?q=semnan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 491: semnan
    http://api.openweathermap.org/data/2.5/weather?q=nguiu&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 492: nguiu
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=lake%20havasu%20city&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 492: lake havasu city
    http://api.openweathermap.org/data/2.5/weather?q=gazojak&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 493: gazojak
    http://api.openweathermap.org/data/2.5/weather?q=saleaula&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 494: saleaula
    Missing result... skipping.
    http://api.openweathermap.org/data/2.5/weather?q=touros&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 494: touros
    http://api.openweathermap.org/data/2.5/weather?q=komsomolskiy&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 495: komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?q=roald&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 496: roald
    http://api.openweathermap.org/data/2.5/weather?q=svetlogorsk&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 497: svetlogorsk
    http://api.openweathermap.org/data/2.5/weather?q=ak-dovurak&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 498: ak-dovurak
    http://api.openweathermap.org/data/2.5/weather?q=biloela&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 499: biloela
    http://api.openweathermap.org/data/2.5/weather?q=grindavik&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 500: grindavik
    http://api.openweathermap.org/data/2.5/weather?q=morki&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 501: morki
    http://api.openweathermap.org/data/2.5/weather?q=clyde%20river&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 502: clyde river
    http://api.openweathermap.org/data/2.5/weather?q=half%20moon%20bay&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 503: half moon bay
    http://api.openweathermap.org/data/2.5/weather?q=bon%20accord&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 504: bon accord
    http://api.openweathermap.org/data/2.5/weather?q=veraval&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 505: veraval
    http://api.openweathermap.org/data/2.5/weather?q=playa%20del%20carmen&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 506: playa del carmen
    http://api.openweathermap.org/data/2.5/weather?q=beidao&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 507: beidao
    http://api.openweathermap.org/data/2.5/weather?q=trairi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 508: trairi
    http://api.openweathermap.org/data/2.5/weather?q=orneta&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 509: orneta
    http://api.openweathermap.org/data/2.5/weather?q=otradnoye&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 510: otradnoye
    http://api.openweathermap.org/data/2.5/weather?q=muros&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 511: muros
    http://api.openweathermap.org/data/2.5/weather?q=itarema&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 512: itarema
    http://api.openweathermap.org/data/2.5/weather?q=mount%20isa&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 513: mount isa
    http://api.openweathermap.org/data/2.5/weather?q=ambovombe&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 514: ambovombe
    http://api.openweathermap.org/data/2.5/weather?q=sheridan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 515: sheridan
    http://api.openweathermap.org/data/2.5/weather?q=mayaguez&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 516: mayaguez
    http://api.openweathermap.org/data/2.5/weather?q=srivardhan&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 517: srivardhan
    http://api.openweathermap.org/data/2.5/weather?q=wenling&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 518: wenling
    http://api.openweathermap.org/data/2.5/weather?q=razole&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 519: razole
    http://api.openweathermap.org/data/2.5/weather?q=omboue&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 520: omboue
    http://api.openweathermap.org/data/2.5/weather?q=west%20wendover&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 521: west wendover
    http://api.openweathermap.org/data/2.5/weather?q=shubarshi&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 522: shubarshi
    http://api.openweathermap.org/data/2.5/weather?q=ewa%20beach&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 523: ewa beach
    http://api.openweathermap.org/data/2.5/weather?q=north%20platte&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 524: north platte
    http://api.openweathermap.org/data/2.5/weather?q=rania&APPID=87cc8d43b5a70c9eea094377511c7804&units=Imperial
    Retrieving Results for city 525: rania
    


```python
#data frame
weather_df = pd.DataFrame({'city':valid_cities,
                           'latitude':lat,
                           'longitude':lng,
                           'temp':temp,
                           'humidity':humidity,
                           'cloudiness':clouds,
                           'wind speeds':wind})


# export data to csv file
weather_df.to_csv("weather_df.csv",index=False)

weather_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>cloudiness</th>
      <th>humidity</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>temp</th>
      <th>wind speeds</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>arica</td>
      <td>75</td>
      <td>72</td>
      <td>-18.48</td>
      <td>-70.32</td>
      <td>60.67</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>leningradskiy</td>
      <td>36</td>
      <td>99</td>
      <td>69.38</td>
      <td>178.42</td>
      <td>32.23</td>
      <td>10.20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>rikitea</td>
      <td>100</td>
      <td>100</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>69.76</td>
      <td>17.69</td>
    </tr>
    <tr>
      <th>3</th>
      <td>vaini</td>
      <td>92</td>
      <td>100</td>
      <td>15.34</td>
      <td>74.49</td>
      <td>69.04</td>
      <td>3.04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>namibe</td>
      <td>8</td>
      <td>100</td>
      <td>-15.19</td>
      <td>12.15</td>
      <td>67.60</td>
      <td>3.94</td>
    </tr>
  </tbody>
</table>
</div>



### scatter plots
temperature(F) vs Latitude
(graphed with latitude on y-axis to correspond with how it runs horizontally )


```python
cmap = cm.get_cmap('jet')
plt.figure(figsize=(20,10))
plt.scatter(weather_df['temp'], weather_df['latitude'], c=weather_df['temp'], cmap=cmap);
plt.title("Temperature (F) vs Latitude on: " + str(datetime.date.today()), fontsize=20)
plt.ylabel("Latitude", fontsize=14)
plt.xlabel('Temperature (degrees F)', fontsize=14)
plt.ylim([-90, 90])

plt.savefig("Temp_v_Lat.png")
plt.show
```




    <function matplotlib.pyplot.show>




![Alt text](Temp_v_Lat.png?raw=true "Optional Title")


Humidity vs Latitdude
(graphed with latitude on y-axis to correspond with how it runs horizontally )


```python
plt.figure(figsize=(20,10))
plt.scatter(weather_df['humidity'], weather_df['latitude']);
plt.title("Humidity (%) vs Latitude on: " + str(datetime.date.today()), fontsize=20)
plt.xlabel("Humidity(%)", fontsize=14)
plt.ylim([-90, 90])
plt.ylabel('Latitude', fontsize=14)
plt.savefig("Humidity.png")
plt.show
```




    <function matplotlib.pyplot.show>




![Alt text](Humidity.png?raw=true "Optional Title")


Cloudiness vs Latitude
(graphed with latitude on y-axis to correspond with how it runs horizontally )


```python
plt.figure(figsize=(20,10))
plt.scatter(weather_df['cloudiness'], weather_df['latitude']);
plt.title("Cloudiness (mph) vs Latitude on: " + str(datetime.date.today()), fontsize=20)
plt.xlabel("Cloudiness (%)", fontsize=14)
plt.ylabel('Latitude', fontsize=14)
plt.ylim([-90,90])
plt.savefig("clouds_v_Lat.png")
plt.show
```




    <function matplotlib.pyplot.show>




![Alt text](clouts_v_Lat.png?raw=true "Optional Title")


Wind Speed vs Latitude
(graphed with latitude on y-axis to correspond with how it runs horizontally )


```python
plt.figure(figsize=(20,10))
plt.scatter(weather_df['wind speeds'], weather_df['latitude']);
plt.title("Wind Speed (mph) vs Latitude on: " + str(datetime.date.today()), fontsize=20)
plt.xlabel("Wind Speed (mph)", fontsize=14)
plt.ylabel('Latitude', fontsize=14)
plt.ylim([-90, 90])
plt.savefig("wind_v_Lat.png")
plt.show
```




    <function matplotlib.pyplot.show>




![Alt text](wind_v_Lat.png?raw=true "Optional Title")



```python
import gmaps
locations = list(zip(weather_df.latitude, weather_df.longitude))
figure_layout ={'width': '1000px', 'height': '500px'}
fig = gmaps.figure(zoom_level=2, center =[39,34], layout=figure_layout)
markers=gmaps.symbol_layer(locations, fill_color='yellow', stroke_color="yellow", scale =3)
fig.add_layer(markers)
fig
```


<p>Failed to display Jupyter Widget of type <code>Figure</code>.</p>
<p>
  If you're reading this message in the Jupyter Notebook or JupyterLab Notebook, it may mean
  that the widgets JavaScript is still loading. If this message persists, it
  likely means that the widgets JavaScript library is either not installed or
  not enabled. See the <a href="https://ipywidgets.readthedocs.io/en/stable/user_install.html">Jupyter
  Widgets Documentation</a> for setup instructions.
</p>
<p>
  If you're reading this message in another frontend (for example, a static
  rendering on GitHub or <a href="https://nbviewer.jupyter.org/">NBViewer</a>),
  it may mean that your frontend doesn't currently support widgets.
</p>



### Observations

1. There is a direct correlation between latitude and temperature. Locations closer to the equator (Lat 0) are warmer than those further from it.
2. Locations north of the equator appear to be slightly more humid than those south of the equator.
3. there is not any noticable correlation between wind speeds and Latitude or cloudiness and Latitude.
