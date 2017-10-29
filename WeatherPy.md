
# WeatherPy - Miriam Berkowitz

## Analysis
### 1) Temperatures are higher closer to the equator.
### 2) Cloudiness is pretty evenly spread across latitudes.
### 3) Wind is calmer closer to the equator.


```python
# 1) Get random coordinates and find the closest city from citypy 
# 2) Use the weather API to get the weather data; ensure no duplicates
# 3) Store the weather data in lists 
# 4) Store the lists in a Data Frame and save in a CSV file
# 5) Plot 4 scatter plots using the Data Frame and save the images
```


```python
# Dependencies
from citipy import citipy
import pandas as pd
from random import uniform
import csv
import matplotlib.pyplot as plt
import requests as req
import seaborn as sns
sns.set(color_codes=True)
from datetime import datetime 

```


```python
# Save config information.
api_key = "c1a253080c68be852f657f9e08b1bd39"
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"

# Build partial query URL
query_url = url + "appid=" + api_key + "&units=" + units + '&q='
```


```python
#initialize variables and lists

cities_needed = 500 
cities_with_data = 0
duplicates = []
cities_without_data = []

lat_data=[]
temp_data=[]
humidity_data=[]
wind_data=[]
city_name=[]
city_id=[]
cloud_data=[]
country_code_data=[]
```


```python
#1) Get cities for random coordinates from citypy 
while (cities_with_data < cities_needed):
           
       
    #get random coordinates; restrict range to get land vs water
    lat = uniform(-45,85)
    long = uniform(-140, 150)
    
    #get the city closest to the coordinate
    city_p=citipy.nearest_city(lat,long)    
    
    #store the nearest city in lower case
    city = str.lower(city_p.city_name)
    country = city_p.country_code
      
    
    #2) ensure the city is not a duplicate of: a)existing list b) cities without weather data or c) duplicates
    if (city not in city_name) and (city not in cities_without_data) and (city not in duplicates):
                   
        #2) get weather data for the city and print log
        total_url = query_url + "city=" +  city + "," + country
        print("city: " + city + ", " + country + ", URL: " + total_url)
        response = req.get(total_url).json()     
                
              
        #3) read and store response data if it exists, and handle missing data
        try:

            # Extract required data from responses and add to data lists
            retrieved_city = str.lower(response.get("name"))
            #print("got data for " + retrieved_city)

            #need to check again for duplicate city because sometimes city returned in response is not
            # exactly the same as the city input into the URL
            if (retrieved_city not in city_name) and (retrieved_city not in duplicates):

                lat_data.append(response.get("coord").get("lat"))
                temp_data.append(response.get("main").get("temp"))
                humidity_data.append(response.get("main").get("humidity"))
                wind_data.append(response.get("wind").get("speed"))
                city_name.append(retrieved_city)
                city_id.append(response.get("id"))
                cloud_data.append(response.get("clouds").get("all"))
                country_code_data.append(response.get("sys").get("country"))
                #date = response.get("dt")
                cities_with_data = cities_with_data + 1
                
            else:
                print("Duplicate in response for: " + retrieved_city)
                duplicates.append(city)
        except:
            print("no data for %s" % city)
            cities_without_data.append(city)
            continue
   
    else:
        print("duplicate: " + city)
        duplicates.append(city)
    #end if    
   
    
#end while loop to get all the cities 


print("cities with data: %s" % str(cities_with_data))
print("cities without data: %s" % len(cities_without_data))
print("dupicates: %s" % len(duplicates))
    
```

    city: attawapiskat, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=attawapiskat,ca
    city: bakchar, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bakchar,ru
    city: hambantota, lk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hambantota,lk
    city: banda aceh, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=banda aceh,id
    city: jacqueville, ci, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jacqueville,ci
    no data for jacqueville
    city: san jose, gt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=san jose,gt
    city: illoqqortoormiut, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=illoqqortoormiut,gl
    no data for illoqqortoormiut
    city: abu jubayhah, sd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=abu jubayhah,sd
    no data for abu jubayhah
    city: belushya guba, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=belushya guba,ru
    city: doha, kw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=doha,kw
    city: barreiras, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=barreiras,br
    city: lebu, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lebu,cl
    city: zabid, ye, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=zabid,ye
    no data for zabid
    city: yellowknife, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yellowknife,ca
    city: bonthe, sl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bonthe,sl
    city: melilla, es, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=melilla,es
    city: cape town, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cape town,za
    city: cayenne, gf, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cayenne,gf
    city: ucluelet, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ucluelet,ca
    city: matagami, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=matagami,ca
    city: sao jose da coroa grande, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sao jose da coroa grande,br
    city: shwebo, mm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=shwebo,mm
    city: tingi, tz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tingi,tz
    city: rikitea, pf, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=rikitea,pf
    no data for rikitea
    city: lagoa, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lagoa,pt
    city: atuona, pf, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=atuona,pf
    city: sao filipe, cv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sao filipe,cv
    no data for sao filipe
    city: pustoshka, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pustoshka,ru
    city: puerto ayora, ec, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=puerto ayora,ec
    city: laguna, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=laguna,br
    city: atuona, pf, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=atuona,pf
    Duplicate in response for: kolonaki
    city: half moon bay, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=half moon bay,us
    city: jamestown, sh, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jamestown,sh
    no data for jamestown
    city: gigmoto, ph, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=gigmoto,ph
    city: saint-ambroise, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint-ambroise,ca
    city: nanortalik, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nanortalik,gl
    no data for nanortalik
    city: charters towers, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=charters towers,au
    city: taolanaro, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=taolanaro,mg
    city: nizhnevartovsk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nizhnevartovsk,ru
    city: ilhabela, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ilhabela,br
    city: burica, pa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=burica,pa
    city: bathsheba, bb, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bathsheba,bb
    no data for bathsheba
    city: rio grande, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=rio grande,br
    city: georgetown, sh, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=georgetown,sh
    no data for georgetown
    duplicate: puerto ayora
    city: busselton, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=busselton,au
    city: lebedyan, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lebedyan,ru
    city: martapura, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=martapura,id
    city: oranjemund, na, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=oranjemund,na
    city: kamenka, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kamenka,ru
    no data for kamenka
    city: ribeira grande, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ribeira grande,pt
    duplicate: yellowknife
    city: kankan, gn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kankan,gn
    city: verkhoyansk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=verkhoyansk,ru
    city: juba, sd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=juba,sd
    city: havre-saint-pierre, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=havre-saint-pierre,ca
    duplicate: jamestown
    city: siocon, ph, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=siocon,ph
    city: bonthe, sl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bonthe,sl
    Duplicate in response for: mogbwemo
    duplicate: cape town
    city: kavaratti, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kavaratti,in
    no data for kavaratti
    city: methoni, gr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=methoni,gr
    city: sisimiut, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sisimiut,gl
    no data for sisimiut
    city: inverness, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=inverness,ca
    duplicate: rikitea
    duplicate: illoqqortoormiut
    city: tromso, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tromso,no
    city: barentsburg, sj, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=barentsburg,sj
    no data for barentsburg
    city: dzhusaly, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dzhusaly,kz
    no data for dzhusaly
    city: espinosa, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=espinosa,br
    duplicate: ribeira grande
    city: tsihombe, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tsihombe,mg
    city: bengkulu, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bengkulu,id
    city: danielskuil, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=danielskuil,za
    duplicate: rikitea
    city: holetown, bb, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=holetown,bb
    city: codrington, ag, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=codrington,ag
    no data for codrington
    city: preston, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=preston,us
    city: kargasok, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kargasok,ru
    city: havoysund, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=havoysund,no
    city: rocky mountain house, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=rocky mountain house,ca
    city: raudeberg, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=raudeberg,no
    duplicate: busselton
    city: nogliki, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nogliki,ru
    city: westport, ie, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=westport,ie
    city: clyde river, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=clyde river,ca
    city: yar-sale, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yar-sale,ru
    city: puerto escondido, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=puerto escondido,mx
    city: belushya guba, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=belushya guba,ru
    Duplicate in response for: varnek
    city: cap malheureux, mu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cap malheureux,mu
    duplicate: sao filipe
    city: ongandjera, na, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ongandjera,na
    no data for ongandjera
    city: lima, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lima,pe
    city: fortuna, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fortuna,us
    city: sabirabad, az, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sabirabad,az
    city: bengkulu, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bengkulu,id
    Duplicate in response for: propinsi bengkulu
    city: san martin, co, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=san martin,co
    city: thompson, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=thompson,ca
    city: mehamn, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mehamn,no
    duplicate: thompson
    city: attawapiskat, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=attawapiskat,ca
    Duplicate in response for: kashechewan
    city: mercedes, ar, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mercedes,ar
    city: srivardhan, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=srivardhan,in
    no data for srivardhan
    city: chokurdakh, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=chokurdakh,ru
    city: virginia beach, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=virginia beach,us
    city: san patricio, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=san patricio,mx
    city: grindavik, is, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=grindavik,is
    city: goderich, sl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=goderich,sl
    city: lebu, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lebu,cl
    Duplicate in response for: britanski trg
    city: hithadhoo, mv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hithadhoo,mv
    city: hobyo, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hobyo,so
    no data for hobyo
    city: bayshore gardens, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bayshore gardens,us
    city: khatanga, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=khatanga,ru
    city: tasiilaq, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tasiilaq,gl
    no data for tasiilaq
    city: meyungs, pw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=meyungs,pw
    no data for meyungs
    city: katsuura, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=katsuura,jp
    no data for katsuura
    duplicate: lebu
    city: kruisfontein, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kruisfontein,za
    duplicate: jamestown
    city: kasongo, cd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kasongo,cd
    city: prince rupert, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=prince rupert,ca
    city: tromso, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tromso,no
    Duplicate in response for: tromsø
    city: teya, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=teya,ru
    no data for teya
    city: sorong, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sorong,id
    duplicate: hithadhoo
    city: tuktoyaktuk, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tuktoyaktuk,ca
    city: vila do maio, cv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vila do maio,cv
    no data for vila do maio
    city: kenora, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kenora,ca
    duplicate: bengkulu
    duplicate: rikitea
    city: iwaki, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=iwaki,jp
    city: peniche, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=peniche,pt
    duplicate: charters towers
    city: urumqi, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=urumqi,cn
    no data for urumqi
    city: saint-philippe, re, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint-philippe,re
    city: tessalit, ml, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tessalit,ml
    city: cairns, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cairns,au
    city: hofn, is, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hofn,is
    no data for hofn
    city: arraial do cabo, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=arraial do cabo,br
    city: bonfim, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bonfim,br
    duplicate: busselton
    city: chagda, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=chagda,ru
    city: tsumeb, na, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tsumeb,na
    duplicate: thompson
    duplicate: rikitea
    city: kourou, gf, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kourou,gf
    duplicate: barentsburg
    city: lagos, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lagos,pt
    city: talnakh, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=talnakh,ru
    city: talavera, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=talavera,pe
    no data for talavera
    duplicate: belushya guba
    duplicate: grindavik
    city: yarada, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yarada,in
    Duplicate in response for: quebec
    duplicate: attawapiskat
    city: naze, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=naze,jp
    no data for naze
    city: cidreira, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cidreira,br
    city: calbuco, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=calbuco,cl
    city: hyeres, fr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hyeres,fr
    city: juneau, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=juneau,us
    city: honningsvag, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=honningsvag,no
    city: mehamn, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mehamn,no
    Duplicate in response for: gamvik
    duplicate: busselton
    city: mahebourg, mu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mahebourg,mu
    city: ankang, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ankang,cn
    no data for ankang
    city: broome, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=broome,au
    city: opobo, ng, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=opobo,ng
    duplicate: matagami
    city: mount gambier, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mount gambier,au
    city: kathmandu, np, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kathmandu,np
    duplicate: rikitea
    duplicate: naze
    city: sinnamary, gf, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sinnamary,gf
    city: grand river south east, mu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=grand river south east,mu
    city: saskylakh, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saskylakh,ru
    city: mrirt, ma, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mrirt,ma
    city: qaanaaq, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=qaanaaq,gl
    no data for qaanaaq
    city: bhongir, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bhongir,in
    city: carnarvon, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=carnarvon,au
    city: manokwari, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=manokwari,id
    city: umm lajj, sa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=umm lajj,sa
    city: hakkari, tr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hakkari,tr
    duplicate: barentsburg
    city: quatre cocos, mu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=quatre cocos,mu
    city: port hardy, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port hardy,ca
    duplicate: mahebourg
    city: lipin bor, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lipin bor,ru
    city: hasaki, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hasaki,jp
    no data for hasaki
    duplicate: puerto ayora
    city: catano, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=catano,us
    city: cockburn town, bs, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cockburn town,bs
    duplicate: atuona
    city: saldanha, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saldanha,za
    duplicate: illoqqortoormiut
    city: helong, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=helong,cn
    no data for helong
    city: jinchengjiang, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jinchengjiang,cn
    no data for jinchengjiang
    city: bambous virieux, mu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bambous virieux,mu
    city: muros, es, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=muros,es
    city: vila franca do campo, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vila franca do campo,pt
    city: jalu, ly, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jalu,ly
    duplicate: thompson
    duplicate: rikitea
    city: miles city, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=miles city,us
    duplicate: atuona
    city: norman wells, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=norman wells,ca
    city: bambanglipuro, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bambanglipuro,id
    duplicate: qaanaaq
    city: ponta do sol, cv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ponta do sol,cv
    duplicate: illoqqortoormiut
    city: quesnel, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=quesnel,ca
    city: port moresby, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port moresby,pg
    duplicate: talnakh
    city: alamogordo, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=alamogordo,us
    duplicate: arraial do cabo
    city: marienburg, sr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=marienburg,sr
    city: ponta delgada, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ponta delgada,pt
    city: shugurovo, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=shugurovo,ru
    city: ixtapa, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ixtapa,mx
    city: sitka, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sitka,us
    city: mayumba, ga, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mayumba,ga
    no data for mayumba
    duplicate: yar-sale
    city: bratsk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bratsk,ru
    city: iquique, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=iquique,cl
    duplicate: muros
    duplicate: belushya guba
    city: mbour, sn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mbour,sn
    city: sayat, tm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sayat,tm
    no data for sayat
    city: taltal, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=taltal,cl
    city: gimli, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=gimli,ca
    city: osa, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=osa,ru
    no data for osa
    city: douentza, ml, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=douentza,ml
    duplicate: ponta do sol
    city: vardo, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vardo,no
    city: fuerte olimpo, py, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fuerte olimpo,py
    city: tiksi, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tiksi,ru
    city: dikson, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dikson,ru
    city: guerrero negro, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=guerrero negro,mx
    city: santa luzia, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=santa luzia,br
    city: northam, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=northam,au
    city: hudson bay, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hudson bay,ca
    city: hamilton, bm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hamilton,bm
    duplicate: ponta do sol
    city: khatanga, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=khatanga,ru
    Duplicate in response for: dutse
    city: ghedi, it, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ghedi,it
    city: chabahar, ir, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=chabahar,ir
    no data for chabahar
    duplicate: norman wells
    duplicate: lebu
    city: cancun, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cancun,mx
    city: dangriga, bz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dangriga,bz
    city: mount isa, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mount isa,au
    city: vangaindrano, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vangaindrano,mg
    duplicate: puerto ayora
    city: upernavik, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=upernavik,gl
    no data for upernavik
    city: maniitsoq, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=maniitsoq,gl
    no data for maniitsoq
    duplicate: banda aceh
    duplicate: rikitea
    city: dikson, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dikson,ru
    Duplicate in response for: dickson city
    duplicate: ribeira grande
    duplicate: saint-philippe
    city: amapa, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=amapa,br
    city: tidore, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tidore,id
    city: nyurba, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nyurba,ru
    no data for nyurba
    city: derzhavinsk, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=derzhavinsk,kz
    city: taolanaro, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=taolanaro,mg
    Duplicate in response for: amparihy
    duplicate: rikitea
    city: fukue, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fukue,jp
    no data for fukue
    city: osmena, ph, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=osmena,ph
    city: rokytne, ua, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=rokytne,ua
    city: port blair, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port blair,in
    city: tual, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tual,id
    city: qasigiannguit, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=qasigiannguit,gl
    no data for qasigiannguit
    city: saint anthony, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint anthony,ca
    city: huarmey, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=huarmey,pe
    duplicate: tiksi
    city: rolim de moura, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=rolim de moura,br
    city: baherden, tm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=baherden,tm
    no data for baherden
    city: sonoita, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sonoita,mx
    city: tazovskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tazovskiy,ru
    city: jaisinghnagar, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jaisinghnagar,in
    city: saint-michel-des-saints, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint-michel-des-saints,ca
    city: zmievka, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=zmievka,ru
    duplicate: ixtapa
    city: deputatskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=deputatskiy,ru
    duplicate: saskylakh
    duplicate: puerto ayora
    duplicate: lebu
    city: fort saint james, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fort saint james,ca
    city: acapulco, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=acapulco,mx
    duplicate: tasiilaq
    city: bayan, kw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bayan,kw
    city: formoso do araguaia, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=formoso do araguaia,br
    city: skoghall, se, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=skoghall,se
    city: labuhan, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=labuhan,id
    no data for labuhan
    city: rapid valley, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=rapid valley,us
    city: shatrovo, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=shatrovo,ru
    city: phalombe, mw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=phalombe,mw
    duplicate: khatanga
    duplicate: puerto ayora
    city: sorvag, fo, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sorvag,fo
    no data for sorvag
    city: lompoc, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lompoc,us
    city: domat, ch, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=domat,ch
    no data for domat
    duplicate: rikitea
    duplicate: barentsburg
    city: maragogi, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=maragogi,br
    city: palmerston, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=palmerston,au
    duplicate: cidreira
    city: zilair, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=zilair,ru
    no data for zilair
    duplicate: barentsburg
    city: san carlos, uy, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=san carlos,uy
    city: djenne, ml, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=djenne,ml
    city: nizhneyansk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nizhneyansk,ru
    city: yecla, es, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yecla,es
    city: santa marta, co, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=santa marta,co
    duplicate: jamestown
    city: tecoanapa, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tecoanapa,mx
    city: tsihombe, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tsihombe,mg
    Duplicate in response for: tsiombe
    duplicate: barentsburg
    city: harrisburg, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=harrisburg,us
    city: coahuayana, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=coahuayana,mx
    city: najran, sa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=najran,sa
    city: kuala pilah, my, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kuala pilah,my
    city: torbay, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=torbay,ca
    duplicate: fortuna
    city: okhotsk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=okhotsk,ru
    duplicate: rikitea
    city: villa carlos paz, ar, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=villa carlos paz,ar
    city: souillac, mu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=souillac,mu
    no data for souillac
    city: diamantino, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=diamantino,br
    city: sarkand, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sarkand,kz
    city: geraldton, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=geraldton,au
    city: kribi, cm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kribi,cm
    duplicate: lompoc
    city: selenice, al, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=selenice,al
    duplicate: puerto ayora
    city: pacific grove, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pacific grove,us
    city: flin flon, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=flin flon,ca
    city: riaba, gq, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=riaba,gq
    duplicate: batagay
    duplicate: hithadhoo
    city: gazanjyk, tm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=gazanjyk,tm
    no data for gazanjyk
    city: bardsir, ir, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bardsir,ir
    no data for bardsir
    duplicate: rikitea
    duplicate: yellowknife
    city: kingsburg, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kingsburg,us
    city: vila velha, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vila velha,br
    duplicate: saskylakh
    duplicate: rikitea
    city: port shepstone, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port shepstone,za
    city: talara, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=talara,pe
    duplicate: torbay
    city: bulolo, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bulolo,pg
    no data for bulolo
    city: geraldton, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=geraldton,au
    Duplicate in response for: utakarra
    city: hay river, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hay river,ca
    duplicate: lebu
    city: hami, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hami,cn
    city: ponta delgada, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ponta delgada,pt
    Duplicate in response for: ponta delgada (são josé)
    city: ancud, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ancud,cl
    city: namyslow, pl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=namyslow,pl
    city: bambanglipuro, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bambanglipuro,id
    Duplicate in response for: pandak
    city: motygino, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=motygino,ru
    city: planaltina, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=planaltina,br
    duplicate: tasiilaq
    city: verkhoyansk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=verkhoyansk,ru
    Duplicate in response for: batagay
    city: burnie, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=burnie,au
    duplicate: rikitea
    city: jawa, jo, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jawa,jo
    duplicate: jamestown
    duplicate: broome
    duplicate: nanortalik
    city: jumla, np, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jumla,np
    city: chimbarongo, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=chimbarongo,cl
    duplicate: port hardy
    duplicate: arraial do cabo
    city: khed, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=khed,in
    city: san rafael, ar, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=san rafael,ar
    duplicate: rikitea
    duplicate: half moon bay
    city: lorengau, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lorengau,pg
    no data for lorengau
    duplicate: talnakh
    city: fairhope, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fairhope,us
    city: saint-joseph, re, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint-joseph,re
    city: mitsamiouli, km, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mitsamiouli,km
    no data for mitsamiouli
    city: lazaro cardenas, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lazaro cardenas,mx
    city: san patricio, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=san patricio,mx
    Duplicate in response for: polanco
    city: senador jose porfirio, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=senador jose porfirio,br
    duplicate: rikitea
    city: pauini, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pauini,br
    city: yantal, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yantal,ru
    city: karaul, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=karaul,ru
    city: ayabe, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ayabe,jp
    city: aviles, es, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aviles,es
    city: mercedes, ar, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mercedes,ar
    Duplicate in response for: monte castro
    city: salalah, om, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=salalah,om
    duplicate: bathsheba
    city: saint george, bm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint george,bm
    duplicate: atuona
    duplicate: fukue
    duplicate: ribeira grande
    city: esna, eg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=esna,eg
    duplicate: belushya guba
    city: matara, lk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=matara,lk
    duplicate: tsihombe
    duplicate: talnakh
    city: ishigaki, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ishigaki,jp
    no data for ishigaki
    city: dingle, ie, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dingle,ie
    duplicate: jamestown
    duplicate: tasiilaq
    city: sfantu gheorghe, ro, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sfantu gheorghe,ro
    duplicate: port hardy
    city: cururupu, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cururupu,br
    city: vestmannaeyjar, is, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vestmannaeyjar,is
    city: oyem, ga, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=oyem,ga
    no data for oyem
    city: najran, sa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=najran,sa
    Duplicate in response for: al mikhlāf
    duplicate: attawapiskat
    city: constitucion, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=constitucion,mx
    city: samarai, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=samarai,pg
    city: baracoa, cu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=baracoa,cu
    city: henties bay, na, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=henties bay,na
    city: katherine, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=katherine,au
    city: cootamundra, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cootamundra,au
    city: pemagatsel, bt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pemagatsel,bt
    no data for pemagatsel
    city: narsaq, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=narsaq,gl
    city: bar harbor, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bar harbor,us
    duplicate: laguna
    duplicate: dingle
    duplicate: atuona
    city: labutta, mm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=labutta,mm
    no data for labutta
    duplicate: lompoc
    duplicate: puerto ayora
    city: saint-pierre, pm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint-pierre,pm
    no data for saint-pierre
    city: pontal do parana, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pontal do parana,br
    city: batticaloa, lk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=batticaloa,lk
    duplicate: illoqqortoormiut
    duplicate: talnakh
    city: charlestown, kn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=charlestown,kn
    city: stornoway, gb, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=stornoway,gb
    city: tokonou, gn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tokonou,gn
    no data for tokonou
    city: la ronge, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=la ronge,ca
    duplicate: thompson
    duplicate: arraial do cabo
    city: pitkyaranta, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pitkyaranta,ru
    city: namibe, ao, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=namibe,ao
    duplicate: hay river
    city: yerbogachen, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yerbogachen,ru
    city: alta floresta, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=alta floresta,br
    duplicate: lebu
    city: narva-joesuu, ee, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=narva-joesuu,ee
    city: visnes, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=visnes,no
    city: iquitos, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=iquitos,pe
    city: carutapera, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=carutapera,br
    city: ubinskoye, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ubinskoye,ru
    city: mogadishu, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mogadishu,so
    city: sinop, tr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sinop,tr
    city: staryy nadym, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=staryy nadym,ru
    duplicate: jamestown
    duplicate: norman wells
    city: los llanos de aridane, es, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=los llanos de aridane,es
    city: lumphat, kh, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lumphat,kh
    city: paris, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=paris,us
    duplicate: vila franca do campo
    city: chickasha, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=chickasha,us
    duplicate: dikson
    city: rocha, uy, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=rocha,uy
    duplicate: thompson
    city: itanagar, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=itanagar,in
    duplicate: yar-sale
    duplicate: dingle
    city: khonuu, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=khonuu,ru
    city: henties bay, na, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=henties bay,na
    Duplicate in response for: hentiesbaai
    duplicate: thompson
    duplicate: carnarvon
    duplicate: rikitea
    duplicate: saldanha
    city: caravelas, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=caravelas,br
    duplicate: tasiilaq
    duplicate: rikitea
    city: ketchikan, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ketchikan,us
    duplicate: saint-philippe
    duplicate: tsihombe
    city: bintulu, my, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bintulu,my
    city: forio, it, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=forio,it
    duplicate: ribeira grande
    city: borkenes, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=borkenes,no
    city: nara, ml, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nara,ml
    no data for nara
    city: port keats, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port keats,au
    duplicate: fortuna
    city: malwan, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=malwan,in
    city: longyearbyen, sj, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=longyearbyen,sj
    no data for longyearbyen
    city: bom jesus, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bom jesus,br
    duplicate: illoqqortoormiut
    city: corinto, ni, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=corinto,ni
    no data for corinto
    duplicate: hithadhoo
    city: kuche, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kuche,cn
    no data for kuche
    city: taoudenni, ml, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=taoudenni,ml
    no data for taoudenni
    city: cabo san lucas, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cabo san lucas,mx
    city: acarau, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=acarau,br
    city: pangkalanbuun, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pangkalanbuun,id
    city: north bend, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=north bend,us
    Duplicate in response for: charlestown
    city: castro, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=castro,cl
    duplicate: saskylakh
    duplicate: ponta do sol
    city: tvoroyri, fo, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tvoroyri,fo
    city: baiyin, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=baiyin,cn
    no data for baiyin
    duplicate: bathsheba
    duplicate: puerto ayora
    duplicate: georgetown
    city: boynton beach, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=boynton beach,us
    duplicate: vardo
    duplicate: hithadhoo
    duplicate: vardo
    city: bella union, uy, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bella union,uy
    duplicate: khatanga
    city: los cedrales, py, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=los cedrales,py
    city: boende, cd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=boende,cd
    no data for boende
    duplicate: upernavik
    city: khagrachari, bd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=khagrachari,bd
    duplicate: ribeira grande
    city: moorhead, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=moorhead,us
    duplicate: vila franca do campo
    city: wajima, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=wajima,jp
    duplicate: saskylakh
    duplicate: jamestown
    duplicate: atuona
    duplicate: barentsburg
    duplicate: ponta do sol
    city: kangaatsiaq, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kangaatsiaq,gl
    no data for kangaatsiaq
    city: amderma, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=amderma,ru
    duplicate: arraial do cabo
    duplicate: lebu
    city: klaksvik, fo, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=klaksvik,fo
    duplicate: lompoc
    duplicate: cayenne
    duplicate: katsuura
    city: port-gentil, ga, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port-gentil,ga
    duplicate: nanortalik
    duplicate: jamestown
    city: aksu, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aksu,cn
    city: mirnyy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mirnyy,ru
    duplicate: georgetown
    duplicate: bintulu
    city: bereda, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bereda,so
    no data for bereda
    duplicate: hithadhoo
    duplicate: sitka
    duplicate: thompson
    city: muroto, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=muroto,jp
    no data for muroto
    duplicate: carnarvon
    city: phalodi, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=phalodi,in
    city: airai, pw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=airai,pw
    no data for airai
    city: shingu, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=shingu,jp
    duplicate: puerto ayora
    city: marsh harbour, bs, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=marsh harbour,bs
    duplicate: mount isa
    duplicate: khatanga
    city: bartica, gy, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bartica,gy
    city: mandera, ke, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mandera,ke
    city: pushkinskiye gory, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pushkinskiye gory,ru
    city: kutina, hr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kutina,hr
    duplicate: saint-joseph
    duplicate: vila do maio
    duplicate: clyde river
    city: igarka, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=igarka,ru
    no data for igarka
    city: namanyere, tz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=namanyere,tz
    no data for namanyere
    city: santa maria, cv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=santa maria,cv
    no data for santa maria
    duplicate: rikitea
    city: constitucion, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=constitucion,mx
    Duplicate in response for: constitución
    city: kaeng khoi, th, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kaeng khoi,th
    city: garowe, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=garowe,so
    no data for garowe
    city: tamandare, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tamandare,br
    city: san cristobal, ec, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=san cristobal,ec
    Duplicate in response for: constitución
    city: yablonovo, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yablonovo,ru
    city: coquimbo, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=coquimbo,cl
    city: esna, eg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=esna,eg
    Duplicate in response for: grindsted
    city: zhezkazgan, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=zhezkazgan,kz
    city: leningradskiy, tj, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=leningradskiy,tj
    city: newport, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=newport,us
    city: dubbo, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dubbo,au
    city: telma, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=telma,ru
    no data for telma
    duplicate: illoqqortoormiut
    duplicate: tasiilaq
    city: alamosa, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=alamosa,us
    city: marco, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=marco,br
    city: sidi qasim, ma, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sidi qasim,ma
    city: fallon, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fallon,us
    duplicate: kamenka
    city: kamaishi, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kamaishi,jp
    no data for kamaishi
    city: korla, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=korla,cn
    duplicate: broome
    city: greece, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=greece,us
    duplicate: hithadhoo
    city: greenville, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=greenville,us
    duplicate: tiksi
    city: shizunai, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=shizunai,jp
    no data for shizunai
    duplicate: rikitea
    duplicate: carutapera
    duplicate: qaanaaq
    city: ganzhou, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ganzhou,cn
    no data for ganzhou
    city: palabuhanratu, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=palabuhanratu,id
    city: statesboro, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=statesboro,us
    duplicate: upernavik
    city: disna, by, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=disna,by
    city: pringsewu, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pringsewu,id
    city: tatishchevo, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tatishchevo,ru
    city: fenoarivo, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fenoarivo,mg
    city: pimentel, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pimentel,pe
    duplicate: yellowknife
    city: umzimvubu, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=umzimvubu,za
    duplicate: khatanga
    duplicate: iwaki
    city: moron, ve, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=moron,ve
    no data for moron
    duplicate: klaksvik
    duplicate: georgetown
    city: tocopilla, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tocopilla,cl
    duplicate: north bend
    city: sentyabrskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sentyabrskiy,ru
    city: pangnirtung, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pangnirtung,ca
    duplicate: codrington
    city: puerto colombia, co, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=puerto colombia,co
    city: stary sacz, pl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=stary sacz,pl
    city: tecoanapa, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tecoanapa,mx
    Duplicate in response for: ayutla de los libres
    duplicate: taolanaro
    city: hualmay, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hualmay,pe
    duplicate: illoqqortoormiut
    city: goba, et, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=goba,et
    no data for goba
    duplicate: illoqqortoormiut
    duplicate: saldanha
    duplicate: kribi
    city: poso, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=poso,id
    duplicate: hithadhoo
    duplicate: tuktoyaktuk
    city: sawakin, sd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sawakin,sd
    city: sentyabrskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sentyabrskiy,ru
    Duplicate in response for: pyt-yakh
    duplicate: busselton
    duplicate: yellowknife
    duplicate: los llanos de aridane
    duplicate: attawapiskat
    city: hermanus, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hermanus,za
    duplicate: taolanaro
    city: keti bandar, pk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=keti bandar,pk
    no data for keti bandar
    duplicate: puerto ayora
    city: baijiantan, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=baijiantan,cn
    no data for baijiantan
    city: chagda, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=chagda,ru
    Duplicate in response for: madanpur
    city: kindu, cd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kindu,cd
    city: mentok, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mentok,id
    city: conceicao do araguaia, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=conceicao do araguaia,br
    duplicate: georgetown
    duplicate: hofn
    city: port alfred, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port alfred,za
    duplicate: arraial do cabo
    duplicate: barentsburg
    duplicate: dingle
    city: turkmenabat, tm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=turkmenabat,tm
    no data for turkmenabat
    city: harper, lr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=harper,lr
    no data for harper
    duplicate: taoudenni
    duplicate: broome
    duplicate: tiksi
    duplicate: igarka
    city: yulara, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yulara,au
    city: erzin, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=erzin,ru
    city: sidi ali, dz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sidi ali,dz
    city: russellville, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=russellville,us
    duplicate: erzin
    duplicate: rikitea
    duplicate: saint-philippe
    duplicate: lebu
    duplicate: cidreira
    city: xining, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=xining,cn
    no data for xining
    city: sandavagur, fo, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sandavagur,fo
    city: itoman, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=itoman,jp
    no data for itoman
    city: qaqortoq, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=qaqortoq,gl
    no data for qaqortoq
    city: cabedelo, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cabedelo,br
    city: cascais, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cascais,pt
    duplicate: norman wells
    duplicate: mahebourg
    duplicate: alta floresta
    duplicate: tasiilaq
    duplicate: bengkulu
    duplicate: jamestown
    duplicate: jamestown
    city: morant bay, jm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=morant bay,jm
    city: kupang, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kupang,id
    city: lerwick, gb, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lerwick,gb
    duplicate: codrington
    city: amderma, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=amderma,ru
    Duplicate in response for: rudnik
    duplicate: los llanos de aridane
    city: serra, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=serra,br
    duplicate: illoqqortoormiut
    city: bouna, ci, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bouna,ci
    no data for bouna
    duplicate: caravelas
    duplicate: half moon bay
    city: faya, td, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=faya,td
    no data for faya
    duplicate: atuona
    city: meulaboh, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=meulaboh,id
    duplicate: carnarvon
    city: manica, mz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=manica,mz
    duplicate: hithadhoo
    city: andevoranto, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=andevoranto,mg
    no data for andevoranto
    duplicate: baijiantan
    city: olinda, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=olinda,br
    city: broken hill, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=broken hill,au
    duplicate: peniche
    duplicate: honningsvag
    city: pacifica, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pacifica,us
    duplicate: arraial do cabo
    city: pierre, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pierre,us
    duplicate: vila franca do campo
    duplicate: sao filipe
    city: kutum, sd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kutum,sd
    duplicate: port blair
    duplicate: codrington
    city: jian, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jian,cn
    city: dwarka, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dwarka,in
    duplicate: lebu
    duplicate: tuktoyaktuk
    duplicate: bambous virieux
    city: aykhal, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aykhal,ru
    no data for aykhal
    city: jalu, ly, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jalu,ly
    Duplicate in response for: gialo
    duplicate: georgetown
    duplicate: upernavik
    city: kedougou, sn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kedougou,sn
    city: tura, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tura,ru
    no data for tura
    city: sofiysk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sofiysk,ru
    duplicate: rikitea
    duplicate: arraial do cabo
    duplicate: port blair
    duplicate: san cristobal
    duplicate: saint-philippe
    city: hailar, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hailar,cn
    no data for hailar
    duplicate: rikitea
    city: sao joao da barra, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sao joao da barra,br
    duplicate: cayenne
    city: santa vitoria do palmar, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=santa vitoria do palmar,br
    duplicate: illoqqortoormiut
    duplicate: clyde river
    city: uyuni, bo, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=uyuni,bo
    no data for uyuni
    duplicate: lagoa
    city: richards bay, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=richards bay,za
    duplicate: airai
    duplicate: rikitea
    city: buraydah, sa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=buraydah,sa
    duplicate: bengkulu
    duplicate: atuona
    duplicate: dikson
    duplicate: cap malheureux
    duplicate: ponta do sol
    city: alyangula, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=alyangula,au
    duplicate: torbay
    duplicate: yulara
    duplicate: rikitea
    duplicate: saint george
    city: samarai, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=samarai,pg
    Duplicate in response for: espoo
    duplicate: busselton
    city: solovetskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=solovetskiy,ru
    duplicate: muros
    duplicate: jamestown
    city: atar, mr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=atar,mr
    no data for atar
    duplicate: klaksvik
    duplicate: khatanga
    duplicate: ribeira grande
    duplicate: tuktoyaktuk
    city: puquio, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=puquio,pe
    city: iqaluit, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=iqaluit,ca
    city: makat, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=makat,kz
    city: eyl, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=eyl,so
    city: acapulco, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=acapulco,mx
    Duplicate in response for: colonia roma
    duplicate: carnarvon
    city: porto novo, cv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=porto novo,cv
    duplicate: carnarvon
    duplicate: arraial do cabo
    city: nouadhibou, mr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nouadhibou,mr
    duplicate: rocha
    duplicate: bathsheba
    city: monte cristi, do, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=monte cristi,do
    city: kovernino, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kovernino,ru
    duplicate: santa maria
    city: novo cruzeiro, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=novo cruzeiro,br
    duplicate: taolanaro
    duplicate: longyearbyen
    city: setermoen, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=setermoen,no
    city: bredasdorp, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bredasdorp,za
    duplicate: lebu
    duplicate: flin flon
    city: touros, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=touros,br
    duplicate: bambous virieux
    city: labuan, my, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=labuan,my
    duplicate: jamestown
    city: takoradi, gh, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=takoradi,gh
    city: adrar, dz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=adrar,dz
    city: soskovo, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=soskovo,ru
    duplicate: ancud
    duplicate: dikson
    duplicate: jamestown
    duplicate: illoqqortoormiut
    city: legnago, it, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=legnago,it
    duplicate: cayenne
    duplicate: talnakh
    city: soure, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=soure,br
    city: aquiraz, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aquiraz,br
    duplicate: puerto ayora
    duplicate: ancud
    duplicate: jamestown
    duplicate: rikitea
    duplicate: meulaboh
    city: vilcun, cl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vilcun,cl
    city: port lincoln, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port lincoln,au
    duplicate: illoqqortoormiut
    city: mayor pablo lagerenza, py, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mayor pablo lagerenza,py
    duplicate: jamestown
    duplicate: acapulco
    duplicate: dzhusaly
    city: gardelegen, de, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=gardelegen,de
    city: vagur, fo, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vagur,fo
    city: comodoro rivadavia, ar, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=comodoro rivadavia,ar
    duplicate: mehamn
    city: maningrida, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=maningrida,au
    city: nadym, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nadym,ru
    city: seminole, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=seminole,us
    city: kungurtug, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kungurtug,ru
    city: albany, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=albany,au
    duplicate: dingle
    duplicate: fuerte olimpo
    city: ust-omchug, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ust-omchug,ru
    city: ashcroft, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ashcroft,ca
    city: hamada, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hamada,jp
    no data for hamada
    duplicate: barentsburg
    city: muscat, om, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=muscat,om
    duplicate: eyl
    duplicate: saskylakh
    duplicate: taltal
    city: at-bashi, kg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=at-bashi,kg
    duplicate: puerto ayora
    duplicate: bambous virieux
    duplicate: rikitea
    city: port moresby, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port moresby,pg
    Duplicate in response for: saraga
    duplicate: muroto
    duplicate: hobyo
    duplicate: bambous virieux
    duplicate: taoudenni
    city: chapais, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=chapais,ca
    duplicate: hay river
    duplicate: ancud
    city: sioux lookout, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sioux lookout,ca
    city: brawley, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=brawley,us
    city: mareeba, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mareeba,au
    duplicate: lorengau
    city: murmashi, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=murmashi,ru
    duplicate: puerto ayora
    duplicate: bakchar
    duplicate: cidreira
    city: guerrero negro, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=guerrero negro,mx
    Duplicate in response for: adolfo lopez mateos
    duplicate: jamestown
    city: arequipa, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=arequipa,pe
    duplicate: rikitea
    city: sonepur, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sonepur,in
    city: luderitz, na, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=luderitz,na
    city: la palma, pa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=la palma,pa
    duplicate: coquimbo
    city: hilo, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hilo,us
    city: obernai, fr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=obernai,fr
    duplicate: busselton
    duplicate: rikitea
    city: greenwood, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=greenwood,us
    duplicate: vila franca do campo
    city: berdigestyakh, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=berdigestyakh,ru
    city: sibolga, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sibolga,id
    city: tabuk, sa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tabuk,sa
    duplicate: attawapiskat
    city: okitipupa, ng, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=okitipupa,ng
    duplicate: qaanaaq
    duplicate: tsihombe
    duplicate: jalu
    city: tawkar, sd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tawkar,sd
    no data for tawkar
    duplicate: longyearbyen
    city: kankaanpaa, fi, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kankaanpaa,fi
    duplicate: hobyo
    city: ostrovnoy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ostrovnoy,ru
    city: wanning, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=wanning,cn
    no data for wanning
    city: pontianak, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pontianak,id
    duplicate: qaanaaq
    city: grand forks, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=grand forks,us
    duplicate: rikitea
    duplicate: qaanaaq
    duplicate: pangnirtung
    city: aksarka, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aksarka,ru
    duplicate: luderitz
    duplicate: ponta do sol
    city: tres arroyos, ar, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tres arroyos,ar
    city: benguela, ao, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=benguela,ao
    duplicate: labutta
    city: esperance, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=esperance,au
    duplicate: esperance
    duplicate: bengkulu
    city: bosaso, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bosaso,so
    duplicate: georgetown
    duplicate: rikitea
    duplicate: port alfred
    duplicate: puerto ayora
    duplicate: sao filipe
    city: praia, cv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=praia,cv
    duplicate: klaksvik
    duplicate: yellowknife
    duplicate: caravelas
    city: narsaq, gl, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=narsaq,gl
    Duplicate in response for: garden city
    city: bubaque, gw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bubaque,gw
    duplicate: sao filipe
    duplicate: port lincoln
    duplicate: coahuayana
    city: freeport, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=freeport,us
    city: saint anthony, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint anthony,ca
    Duplicate in response for: sainte-foy
    duplicate: caravelas
    duplicate: saskylakh
    duplicate: yellowknife
    city: kidal, ml, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kidal,ml
    city: amapa, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=amapa,br
    Duplicate in response for: maracá
    duplicate: georgetown
    city: vanimo, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vanimo,pg
    no data for vanimo
    city: aflu, dz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aflu,dz
    duplicate: ponta do sol
    duplicate: katsuura
    city: gamba, ga, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=gamba,ga
    duplicate: labuhan
    city: mehriz, ir, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mehriz,ir
    no data for mehriz
    city: fort-shevchenko, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=fort-shevchenko,kz
    city: ayios matthaios, gr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ayios matthaios,gr
    city: riyadh, sa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=riyadh,sa
    city: aswan, eg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aswan,eg
    duplicate: lompoc
    duplicate: quatre cocos
    city: divnomorskoye, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=divnomorskoye,ru
    duplicate: xining
    duplicate: albany
    city: saint-augustin, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=saint-augustin,ca
    Duplicate in response for: quebec
    duplicate: airai
    city: luanda, ao, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=luanda,ao
    duplicate: lagoa
    city: meadow lake, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=meadow lake,ca
    city: hanna, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hanna,ca
    Duplicate in response for: quebec
    duplicate: tiksi
    duplicate: vanimo
    city: camacha, pt, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=camacha,pt
    city: dolores, ph, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dolores,ph
    duplicate: jamestown
    city: sur, om, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=sur,om
    duplicate: lompoc
    duplicate: jamestown
    duplicate: klaksvik
    city: tumannyy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tumannyy,ru
    duplicate: tura
    city: nabire, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nabire,id
    city: ginir, et, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ginir,et
    city: derbent, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=derbent,ru
    city: ardakan, ir, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ardakan,ir
    city: dallas, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dallas,us
    duplicate: illoqqortoormiut
    city: nizhneyansk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nizhneyansk,ru
    Duplicate in response for: nizhnekamsk
    duplicate: ribeira grande
    city: khanpur, pk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=khanpur,pk
    Duplicate in response for: ahmadabad
    duplicate: iquique
    duplicate: la ronge
    city: cozumel, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=cozumel,mx
    Duplicate in response for: colonia roma
    city: bargur, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bargur,in
    no data for bargur
    duplicate: bambous virieux
    duplicate: tamandare
    city: soavinandriana, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=soavinandriana,mg
    city: babynino, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=babynino,ru
    city: yoichi, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=yoichi,jp
    no data for yoichi
    duplicate: hilo
    city: mayor pablo lagerenza, py, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mayor pablo lagerenza,py
    Duplicate in response for: capitan pablo lagerenza
    duplicate: divnomorskoye
    city: mullaitivu, lk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mullaitivu,lk
    city: matara, lk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=matara,lk
    Duplicate in response for: weragampita
    duplicate: saint george
    duplicate: lagoa
    duplicate: bambous virieux
    duplicate: attawapiskat
    duplicate: los llanos de aridane
    duplicate: taoudenni
    duplicate: mahebourg
    duplicate: saint-philippe
    duplicate: vagur
    duplicate: namibe
    duplicate: namibe
    duplicate: mehamn
    city: primorsk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=primorsk,ru
    duplicate: la ronge
    duplicate: cabo san lucas
    duplicate: erzin
    duplicate: tabuk
    city: kurilsk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kurilsk,ru
    no data for kurilsk
    duplicate: rikitea
    city: kachikau, bw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kachikau,bw
    city: jahrom, ir, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jahrom,ir
    city: tanabe, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tanabe,jp
    no data for tanabe
    duplicate: rikitea
    duplicate: san cristobal
    duplicate: chimbarongo
    city: zonguldak, tr, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=zonguldak,tr
    duplicate: vila franca do campo
    duplicate: murmashi
    duplicate: arraial do cabo
    city: hualmay, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hualmay,pe
    Duplicate in response for: huacho
    duplicate: coquimbo
    city: hermon, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hermon,us
    city: mayskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mayskiy,ru
    duplicate: thompson
    duplicate: bintulu
    duplicate: arraial do cabo
    city: bokajan, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bokajan,in
    no data for bokajan
    duplicate: clyde river
    city: tumannyy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tumannyy,ru
    Duplicate in response for: teriberka
    city: mezen, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=mezen,ru
    duplicate: geraldton
    duplicate: acarau
    duplicate: bengkulu
    duplicate: cidreira
    duplicate: hofn
    duplicate: belushya guba
    duplicate: najran
    duplicate: jamestown
    city: szentgotthard, hu, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=szentgotthard,hu
    city: wagar, sd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=wagar,sd
    city: bontang, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bontang,id
    city: palakkodu, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=palakkodu,in
    duplicate: bambous virieux
    city: kazachinskoye, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kazachinskoye,ru
    duplicate: longyearbyen
    duplicate: tual
    city: baoding, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=baoding,cn
    no data for baoding
    city: salinas, ec, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=salinas,ec
    no data for salinas
    city: staryy biser, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=staryy biser,ru
    city: victoria, sc, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=victoria,sc
    duplicate: ribeira grande
    city: kavieng, pg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kavieng,pg
    no data for kavieng
    duplicate: norman wells
    duplicate: puerto ayora
    duplicate: acapulco
    duplicate: tasiilaq
    city: jammalamadugu, in, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jammalamadugu,in
    city: nemuro, jp, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nemuro,jp
    no data for nemuro
    city: dire, ml, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=dire,ml
    city: balykshi, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=balykshi,kz
    no data for balykshi
    city: makhinjauri, ge, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=makhinjauri,ge
    duplicate: atuona
    duplicate: saldanha
    city: ambon, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ambon,id
    duplicate: matara
    duplicate: dikson
    city: kaka, tm, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kaka,tm
    no data for kaka
    duplicate: codrington
    duplicate: batagay
    city: funadhoo, mv, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=funadhoo,mv
    duplicate: illoqqortoormiut
    duplicate: khatanga
    city: bandarbeyla, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bandarbeyla,so
    duplicate: vardo
    duplicate: puerto ayora
    duplicate: oranjemund
    duplicate: fallon
    city: lasa, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=lasa,cn
    city: linqu, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=linqu,cn
    city: tsienyane, bw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tsienyane,bw
    city: atbasar, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=atbasar,kz
    duplicate: tessalit
    city: shaunavon, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=shaunavon,ca
    duplicate: talnakh
    duplicate: port hardy
    city: bargal, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bargal,so
    city: svetlogorsk, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=svetlogorsk,ru
    city: buta, cd, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=buta,cd
    no data for buta
    duplicate: vestmannaeyjar
    duplicate: upernavik
    city: palabuhanratu, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=palabuhanratu,id
    Duplicate in response for: cimaja
    duplicate: lompoc
    city: kloulklubed, pw, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kloulklubed,pw
    no data for kloulklubed
    city: longkou, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=longkou,cn
    no data for longkou
    duplicate: chokurdakh
    city: conde, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=conde,br
    duplicate: tasiilaq
    city: bonfim, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=bonfim,br
    Duplicate in response for: rio manso
    duplicate: rikitea
    city: martapura, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=martapura,id
    Duplicate in response for: baturaja
    city: brooks, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=brooks,ca
    city: jhelum, pk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jhelum,pk
    duplicate: yar-sale
    duplicate: illoqqortoormiut
    city: windhoek, na, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=windhoek,na
    city: simao, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=simao,cn
    no data for simao
    duplicate: airai
    city: port keats, au, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=port keats,au
    Duplicate in response for: port-au-prince
    duplicate: lompoc
    city: tuy hoa, vn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=tuy hoa,vn
    duplicate: tumannyy
    city: hovd, mn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hovd,mn
    no data for hovd
    city: ayagoz, kz, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=ayagoz,kz
    city: toamasina, mg, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=toamasina,mg
    duplicate: sao filipe
    duplicate: laguna
    duplicate: torbay
    city: waynesville, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=waynesville,us
    city: vryburg, za, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=vryburg,za
    duplicate: lagoa
    city: marcona, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=marcona,pe
    city: celestun, mx, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=celestun,mx
    city: waingapu, id, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=waingapu,id
    city: norman, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=norman,us
    duplicate: puerto ayora
    duplicate: victoria
    duplicate: attawapiskat
    duplicate: ribeira grande
    city: suhbaatar, mn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=suhbaatar,mn
    duplicate: chapais
    duplicate: lompoc
    duplicate: arraial do cabo
    duplicate: clyde river
    city: limaong, ph, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=limaong,ph
    city: andenes, no, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=andenes,no
    duplicate: cape town
    duplicate: puerto ayora
    duplicate: prince rupert
    duplicate: thompson
    city: hays, us, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=hays,us
    city: pitimbu, br, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=pitimbu,br
    duplicate: yellowknife
    duplicate: hamilton
    city: jawhar, so, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=jawhar,so
    duplicate: saint-philippe
    duplicate: hobyo
    duplicate: iqaluit
    city: komyshnya, ua, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=komyshnya,ua
    city: doha, qa, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=doha,qa
    city: caxito, ao, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=caxito,ao
    no data for caxito
    city: turan, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=turan,ru
    Duplicate in response for: torino
    city: shumskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=shumskiy,ru
    duplicate: rikitea
    duplicate: tasiilaq
    city: asfi, ma, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=asfi,ma
    no data for asfi
    city: nguru, ng, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=nguru,ng
    city: batticaloa, lk, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=batticaloa,lk
    Duplicate in response for: puliyadikuda
    duplicate: bambous virieux
    city: grand centre, ca, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=grand centre,ca
    duplicate: carnarvon
    duplicate: quatre cocos
    city: aldan, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=aldan,ru
    no data for aldan
    city: deputatskiy, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=deputatskiy,ru
    Duplicate in response for: zaural’skiy
    duplicate: nanortalik
    city: marcona, pe, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=marcona,pe
    Duplicate in response for: san juan
    city: kaifeng, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=kaifeng,cn
    no data for kaifeng
    duplicate: illoqqortoormiut
    duplicate: qaanaaq
    duplicate: yellowknife
    duplicate: ponta do sol
    duplicate: saint-philippe
    city: xiaoshi, cn, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=xiaoshi,cn
    no data for xiaoshi
    duplicate: katsuura
    duplicate: santa maria
    duplicate: tuktoyaktuk
    city: oktyabrskoye, ru, URL: http://api.openweathermap.org/data/2.5/weather?appid=c1a253080c68be852f657f9e08b1bd39&units=imperial&q=city=oktyabrskoye,ru
    cities with data: 500
    cities without data: 121
    dupicates: 534



```python
#4) store the results in a data frame and write to a CSV file

all_data = {"temp": temp_data, "lat": lat_data, "city": city_name, "id": city_id, "country_code": country_code_data,
            "humidity": humidity_data, "windspeed": wind_data, "cloudiness": cloud_data}

final_data = pd.DataFrame(all_data)

#reorder the columns
final_data = final_data[["id", "city", "country_code", "lat", "temp","humidity", "windspeed", "cloudiness" ]]

#write to a CSV file
final_data.to_csv("final_data.csv", index=False, header=True)
final_data.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>country_code</th>
      <th>lat</th>
      <th>temp</th>
      <th>humidity</th>
      <th>windspeed</th>
      <th>cloudiness</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5989520</td>
      <td>kashechewan</td>
      <td>CA</td>
      <td>52.30</td>
      <td>34.41</td>
      <td>68</td>
      <td>3.74</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1511057</td>
      <td>bakchar</td>
      <td>RU</td>
      <td>57.02</td>
      <td>20.95</td>
      <td>76</td>
      <td>6.98</td>
      <td>24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1244926</td>
      <td>hambantota</td>
      <td>LK</td>
      <td>6.12</td>
      <td>79.27</td>
      <td>100</td>
      <td>8.21</td>
      <td>44</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1215502</td>
      <td>banda aceh</td>
      <td>ID</td>
      <td>5.56</td>
      <td>72.16</td>
      <td>100</td>
      <td>1.95</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1689431</td>
      <td>san jose</td>
      <td>PH</td>
      <td>15.79</td>
      <td>74.32</td>
      <td>87</td>
      <td>2.06</td>
      <td>88</td>
    </tr>
  </tbody>
</table>
</div>




```python
#get current date for the plot titles
date = datetime.now().strftime("%m/%d/%y")    
```


```python
# 5) Generate plots using Seaborn

#create TEMPERATURE scatter plot using seaborn

ax = sns.lmplot(x="lat", y="temp", data=final_data, fit_reg=False, legend_out=False,size=8,aspect=1.3)
   
plt.title("World City vs Current Temperature (" + date + ")")
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")

# Save the figure
plt.savefig("TemperatureInWorldCities.png")

plt.show()
```


![png](output_9_0.png)



```python
#create HUMIDITY scatter plot using seaborn
ax = sns.lmplot(x="lat", y="humidity", data=final_data, fit_reg=False, legend_out=False,size=8,aspect=1.3)
        
plt.title("Humidity in World Cities (" + date + ")")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")

# Save the figure
plt.savefig("HumidityInWorldCities.png")

plt.show()
```


![png](output_10_0.png)



```python
#create CLOUDINESS scatter plot using seaborn
ax = sns.lmplot(x="lat", y="cloudiness", data=final_data, fit_reg=False,
        legend_out=False,size=8,aspect=1.3)
        
plt.title("Cloudiness in World Cities (" + date + ")")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
# Save the figure
plt.savefig("CloudinessInWorldCities.png")

plt.show()

```


![png](output_11_0.png)



```python
#create WIND SPEED scatter plot using seaborn
ax = sns.lmplot(x="lat", y="windspeed", data=final_data, fit_reg=False,size=8,aspect=1.3,
        legend_out=False)
        
plt.title("Wind Speed in World Cities (" + date + ")")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")
# Save the figure
plt.savefig("WindSpeedInWorldCities.png")

plt.show()
```


![png](output_12_0.png)



```python
final_data['city'].unique()
```




    array(['kashechewan', 'bakchar', 'hambantota', 'banda aceh', 'san jose',
           'varnek', 'muḩāfaz̧at al ‘āşimah', 'barreiras', 'britanski trg',
           'yellowknife', 'mogbwemo', 'melilla', 'cape town', 'cayenne',
           'ucluelet', 'matagami', 'sao jose da coroa grande', 'meadowlands',
           'san nicolas', 'lagoa', 'kolonaki', 'pustoshka', 'puerto ayora',
           'laguna', 'half moon bay', 'gigmoto', 'quebec', 'charters towers',
           'amparihy', 'nizhnevartovsk', 'ilhabela', 'lenape heights',
           'rio grande', 'busselton', 'pushkari', 'baturaja', 'oranjemund',
           'ribeira grande', 'kankan', 'batagay', 'south sudan',
           'havre-saint-pierre', 'siocon', 'makrygialos',
           'desert hills trailer park', 'tromsø', 'espinosa', 'tsiombe',
           'propinsi bengkulu', 'danielskuil', 'holetown', 'preston city',
           'kargasok', 'havoysund', 'otway', 'maloy', 'nogliki', 'westport',
           'clyde river', 'yar-sale', 'alvaro obregon', 'cap malheureux',
           'rímac', 'fortuna', 'sabirabad', 'san martin', 'thompson', 'gamvik',
           'monte castro', 'chokurdakh', 'city of virginia beach', 'polanco',
           'grindavik', 'goderich', 'hithadhoo', 'tampa', 'dutse', 'rosslyn',
           'kasongo', 'prince rupert', 'sorong', 'tuktoyaktuk', 'kenora',
           'iwaki', 'peniche', 'saint-philippe', 'tessalit', 'cairns',
           'arraial do cabo', 'rio manso', 'madanpur', 'tsumeb', 'kourou',
           'lagos', 'talnakh', 'cidreira', 'calbuco', 'hyeres', 'juneau',
           'honningsvag', 'mahebourg', 'broome', 'yenagoa', 'mount gambier',
           'bāneswar', 'sinnamary', 'grande riviere sud est', 'saskylakh',
           'aguelmous', 'bhongir', 'carnarvon', 'manokwari', 'mecca',
           'hakkari', 'quatre cocos', 'port hardy', 'lipin bor', 'new york',
           'cockburn town', 'saldanha', 'bambous virieux', 'muros',
           'vila franca do campo', 'gialo', 'miles city', 'norman wells',
           'pandak', 'ponta do sol', 'quesnel', 'saraga', 'alamogordo',
           'marienberg', 'ponta delgada (são josé)', 'shugurovo', 'ixtapa',
           'sitka', 'bratsk', 'iquique', 'mbour', 'taltal', 'gimli',
           'douentza', 'vardo', 'fuerte olimpo', 'tiksi', 'dickson city',
           'adolfo lopez mateos', 'santa luzia', 'northam', 'hudson bay',
           'hamilton', 'ghedi', 'alfredo v. bonfil', 'dangriga', 'mount isa',
           'vangaindrano', 'maracá', 'ternate', 'derzhavinsk', 'lope de vega',
           'rokytne', 'port blair', 'tual', 'sainte-foy', 'quilipe',
           'vista alegre', 'sonoyta', 'tazovskiy', 'jaisinghnagar',
           'saint-michel-des-saints', 'zmeyëvka', 'zaural’skiy',
           'fort st. james', 'colonia roma', 'al qiblah', 'gurupi', 'skoghall',
           'rapid valley', 'vyerkhnyadzvinsk', 'phalombe', 'lompoc',
           'maragogi', 'palmerston', 'province of pangasinan', 'djenne',
           'nizhnekamsk', 'yecla', 'santa marta', 'ayutla de los libres',
           'harrisburg', 'coahuayana', 'al mikhlāf', 'kuala pilah', 'torbay',
           'okhotsk', 'villa del lago', 'diamantino', 'svodin', 'utakarra',
           'kribi', 'selenice', 'pacific grove', 'flin flon',
           'welwyn garden city', 'kingsburg', 'vila velha', 'port shepstone',
           'kilo', 'hay river', 'neustadt', 'ancud', 'namyslow', 'zatur’ye',
           'planaltina', 'burnie', 'kotwa', 'jumla', 'chimbarongo',
           'rajgurunagar', 'san rafael', 'fairhope', 'saint-joseph',
           'san pedro zacatenco', 'senador jose porfirio', 'pauini',
           'joinville', 'hindaun', 'ayabe', 'aviles', 'salalah',
           'saint george', 'grindsted', 'weragampita', 'dingle',
           'sfantu-gheorghe', 'cururupu', 'vestmannaeyjar', 'constitución',
           'espoo', 'baracoa', 'hentiesbaai', 'katherine', 'cootamundra',
           'garden city', 'bar harbor', 'pontal do parana', 'puliyadikuda',
           'charlestown', 'stornoway', 'la ronge', 'pitkyaranta', 'namibe',
           'yerbogachen', 'alta floresta', 'narva-joesuu', 'visnes', 'iquitos',
           'carutapera', 'ubinskoye', 'mogadishu', 'sinop', 'staryy nadym',
           'los llanos de aridane', 'lumphat', 'new orleans', 'chickasha',
           'rocha', 'ahmadabad', 'khon kaen', 'caravelas', 'ketchikan',
           'bintulu', 'forio', 'borkenes', 'port-au-prince', 'malvan',
           'bom jesus', 'cabo san lucas', 'acarau', 'pangkalanbuun',
           'san ángel inn', 'tvoroyri', 'boynton beach', 'bella union',
           'los cedrales', 'khagrachhari', 'morehead city', 'ystradowen',
           'rudnik', 'klaksvik', 'port-gentil', 'yeşilköy', 'mirnyy',
           'phalodi', 'fukuma', 'marsh harbour', 'bartica', 'mandera',
           'pushkinskiye gory', 'kutina', 'kaeng khoi', 'tamandare',
           'zarovtsy', 'coquimbo', 'zhezqazghan', 'leningradskaya',
           'city of newport news', 'dubbo', 'union city', 'cellino san marco',
           'mexico city', 'fallon', 'otaniemi', 'greece', 'greenville',
           'cimaja', 'statesboro', 'dzisna', 'pringsewu', 'tatishchevo',
           'fenoarivo atsinanana', 'pimentel', 'kokstad', 'tocopilla',
           'pyt-yakh', 'pangnirtung', 'puerto colombia', 'stary sacz',
           'huacho', 'kasiguncu', 'sawakin', 'hermanus', 'kindu', 'menton',
           'conceicao do araguaia', 'port alfred', 'yulara', 'erzin',
           'wilaya de mostaganem', 'russellville', 'midvagur', 'cabedelo',
           'cascais', 'morant bay', 'oesapa', 'lerwick', 'serra', 'meulaboh',
           'santa cruz', 'olinda', 'broken hill municipality', 'pacific city',
           'pierre', 'kutum', 'sofia', 'dwarka', 'kedougou',
           'mahilyowskaya voblasts’', 'sao joao da barra',
           'santa vitoria do palmar', 'arboretum', 'buraydah', 'alyangula',
           'rabocheostrovsk', 'puquio', 'iqaluit', 'valenzuela', 'eyl',
           'porto-novo', 'nouadhibou', 'erand', 'kovernino', 'são bentinho',
           'setermoen', 'bredasdorp', 'touros', 'labuan', 'takoradi', 'adrar',
           'leonikha', 'legnago', 'soure', 'berra bode', 'vilcun',
           'port lincoln', 'capitan pablo lagerenza', 'gardelegen', 'vagur',
           'comodoro rivadavia', 'maningrida', 'nadym', 'seminole',
           'kungurtug', 'albany', 'ust-omchug', 'ashcroft', 'muscat', 'naryn',
           'chapais', 'sioux lookout', 'brawley', 'mareeba', 'murmashi',
           'arequipa', 'hajipur', 'luderitz', 'palma de mallorca', 'hilo',
           'obernai', 'greenwood', 'berdigestyakh', 'sibolga', 'tabuk',
           'agbabu', 'kankaanpaa', 'ostrovnoy', 'pontianak', 'grand forks',
           'aksarka', 'villa santa rita', 'benguela', 'esperance', 'boosaaso',
           'praia', 'bubaque', 'freeport', 'kidal', 'aflou', 'belhar',
           'fort-shevchenko', 'agios matthaios', 'riyadh', 'aswan',
           'divnomorskoye', 'luanda', 'north fair oaks', 'floresta',
           'del pilar', 'testing', 'teriberka', 'nabire', 'ginir', 'derbent',
           'ardahan', 'dallas', 'soavinandriana', 'babynino',
           'northern province', 'primorsk', 'kosheshe', 'hārom', 'zonguldak',
           'hermon', 'mayskiy', 'mezen', 'szentgotthard', 'onida', 'bontang',
           'palakkodu', 'kazachinskoye', 'sarany', 'victoria', 'jammalamadugu',
           'goundam', 'makhinjauri', 'ambon', 'funadhoo', 'patamda',
           'las pinas', 'torino', 'botswana', 'atbasar', 'shaunavon', 'turku',
           'svetlogorsk', 'conde', 'brooks', 'jhelum', 'windhoek', 'tuy hoa',
           'ayagoz', 'toamasina', 'waynesville', 'vryburg', 'san juan',
           'celestun', 'umburarameha', 'norman', 'baruun-urt', 'limaong',
           'andenes', 'hays', 'pitimbu', 'jawhar', 'kamyshnya', 'doha',
           'shimsk', 'nguru', 'mississauga', 'oktyabrskiy'], dtype=object)




```python
final_data['city'].count()
```




    500


