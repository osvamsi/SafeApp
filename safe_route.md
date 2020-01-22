```python
import pandas as pd
pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)

```


```python
crime_df = pd.read_csv("crime-data_crime-data_crimestat.csv")
```


```python
crime_df.head()
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
      <th>INC NUMBER</th>
      <th>OCCURRED ON</th>
      <th>OCCURRED TO</th>
      <th>UCR CRIME CATEGORY</th>
      <th>100 BLOCK ADDR</th>
      <th>ZIP</th>
      <th>PREMISE TYPE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>201500002101405</td>
      <td>11/01/2015  00:00</td>
      <td>11/01/2015  05:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>102XX W MEDLOCK AVE</td>
      <td>85307.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
    <tr>
      <th>1</th>
      <td>201500002102668</td>
      <td>11/01/2015  00:00</td>
      <td>11/01/2015  11:50</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>69XX W WOOD ST</td>
      <td>85043.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
    <tr>
      <th>2</th>
      <td>201600000052855</td>
      <td>11/01/2015  00:00</td>
      <td>01/09/2016  00:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>N 43RD AVE &amp; W CACTUS RD</td>
      <td>85029.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
    <tr>
      <th>3</th>
      <td>201500002168686</td>
      <td>11/01/2015  00:00</td>
      <td>11/11/2015  09:30</td>
      <td>LARCENY-THEFT</td>
      <td>14XX E HIGHLAND AVE</td>
      <td>85014.0</td>
      <td>PARKING LOT</td>
    </tr>
    <tr>
      <th>4</th>
      <td>201700001722914</td>
      <td>11/01/2015  00:00</td>
      <td>NaN</td>
      <td>LARCENY-THEFT</td>
      <td>279XX N 23RD LN</td>
      <td>85085.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
  </tbody>
</table>
</div>



### Let's clean those column names


```python
col = crime_df.columns.tolist()
for i in range(len(col)):
  temp = col[i]
  temp = temp.lower()
  temp = temp.replace(" ", "_")
  col[i] = temp
  
print(col)
```

    ['inc_number', 'occurred_on', 'occurred_to', 'ucr_crime_category', '100_block_addr', 'zip', 'premise_type']
    


```python
crime_df.columns = col
```

### Converting to datetime object 


```python
crime_df['occurred_on'] = pd.to_datetime(crime_df['occurred_on'])
crime_df['occurred_to'] = pd.to_datetime(crime_df['occurred_to'])
```


```python
crime_df.head()
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
      <th>inc_number</th>
      <th>occurred_on</th>
      <th>occurred_to</th>
      <th>ucr_crime_category</th>
      <th>100_block_addr</th>
      <th>zip</th>
      <th>premise_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>201500002101405</td>
      <td>2015-11-01</td>
      <td>2015-11-01 05:00:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>102XX W MEDLOCK AVE</td>
      <td>85307.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
    <tr>
      <th>1</th>
      <td>201500002102668</td>
      <td>2015-11-01</td>
      <td>2015-11-01 11:50:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>69XX W WOOD ST</td>
      <td>85043.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
    <tr>
      <th>2</th>
      <td>201600000052855</td>
      <td>2015-11-01</td>
      <td>2016-01-09 00:00:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>N 43RD AVE &amp; W CACTUS RD</td>
      <td>85029.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
    <tr>
      <th>3</th>
      <td>201500002168686</td>
      <td>2015-11-01</td>
      <td>2015-11-11 09:30:00</td>
      <td>LARCENY-THEFT</td>
      <td>14XX E HIGHLAND AVE</td>
      <td>85014.0</td>
      <td>PARKING LOT</td>
    </tr>
    <tr>
      <th>4</th>
      <td>201700001722914</td>
      <td>2015-11-01</td>
      <td>NaT</td>
      <td>LARCENY-THEFT</td>
      <td>279XX N 23RD LN</td>
      <td>85085.0</td>
      <td>SINGLE FAMILY HOUSE</td>
    </tr>
  </tbody>
</table>
</div>



### Installing geopy module for converting address to Latitude and Longitude.


```python
!pip install geopy
from geopy.geocoders import Nominatim # convert an address into latitude and longitude values
```

### Usage of geopy module 


```python
address = '10200 W wood st,85043'
geolocator = Nominatim(user_agent="explorer")
location = geolocator.geocode(address)
print(location)
latitude = location.latitude
longitude = location.longitude
print('The geograpical coordinate of 102XX W MEDLOCK AVE,85307.0 {}, {}.'.format(latitude, longitude))
```

    West Wood Street, Santa Maria, Estrella, Phoenix, Maricopa County, Arizona, 85043, United States of America
    The geograpical coordinate of 102XX W MEDLOCK AVE,85307.0 33.4096802, -112.1896586.
    


```python
address = 'W wood st,85043'
geolocator = Nominatim(user_agent="explorer")
location = geolocator.geocode(address)
print(location)
latitude = location.latitude
longitude = location.longitude
print('The geograpical coordinate of 102XX W MEDLOCK AVE,85307.0 {}, {}.'.format(latitude, longitude))
```

    West Wood Street, Sienna Vista, Estrella, Phoenix, Maricopa County, Arizona, 85043, United States of America
    The geograpical coordinate of 102XX W MEDLOCK AVE,85307.0 33.4108509, -112.2191746.
    

### Data cleaning
We need to replace "xx" with some digits to get the accurate location.


```python
def replaceXXwithOO(input, pattern = "XX", replaceWith = "00"): 
    return input.replace(pattern, replaceWith) 
  
```


```python
crime_df['100_block_addr'] = crime_df['100_block_addr'].apply(replaceXXwithOO)
```


```python
crime_df['100_block_addr'].head()
```




    0         10200 W MEDLOCK AVE
    1              6900 W WOOD ST
    2    N 43RD AVE & W CACTUS RD
    3         1400 E HIGHLAND AVE
    4             27900 N 23RD LN
    Name: 100_block_addr, dtype: object



yayy! replaced "xx" with "00". Now we extract Latitude and Longitudes of these addresses.

---




```python
crime_df["Year"]= crime_df["occurred_on"].dt.year
```


```python
crime_df.head()
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
      <th>inc_number</th>
      <th>occurred_on</th>
      <th>occurred_to</th>
      <th>ucr_crime_category</th>
      <th>100_block_addr</th>
      <th>zip</th>
      <th>premise_type</th>
      <th>Year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>201500002101405</td>
      <td>2015-11-01</td>
      <td>2015-11-01 05:00:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>10200 W MEDLOCK AVE</td>
      <td>85307.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>201500002102668</td>
      <td>2015-11-01</td>
      <td>2015-11-01 11:50:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>6900 W WOOD ST</td>
      <td>85043.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>201600000052855</td>
      <td>2015-11-01</td>
      <td>2016-01-09 00:00:00</td>
      <td>MOTOR VEHICLE THEFT</td>
      <td>N 43RD AVE &amp; W CACTUS RD</td>
      <td>85029.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>201500002168686</td>
      <td>2015-11-01</td>
      <td>2015-11-11 09:30:00</td>
      <td>LARCENY-THEFT</td>
      <td>1400 E HIGHLAND AVE</td>
      <td>85014.0</td>
      <td>PARKING LOT</td>
      <td>2015.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>201700001722914</td>
      <td>2015-11-01</td>
      <td>NaT</td>
      <td>LARCENY-THEFT</td>
      <td>27900 N 23RD LN</td>
      <td>85085.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2015.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
crime_df["Month"] = crime_df["occurred_on"].dt.month
```


```python
crime_df["Month"].head()
```




    0    11.0
    1    11.0
    2    11.0
    3    11.0
    4    11.0
    Name: Month, dtype: float64



### we will select crime that happened in 2019


```python
year = ["2019"]
crime2019 = crime_df[crime_df.Year.isin(year)]
```


```python
crime2019.head()
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
      <th>inc_number</th>
      <th>occurred_on</th>
      <th>occurred_to</th>
      <th>ucr_crime_category</th>
      <th>100_block_addr</th>
      <th>zip</th>
      <th>premise_type</th>
      <th>Year</th>
      <th>Month</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>204486</th>
      <td>201900000855221</td>
      <td>2019-01-01</td>
      <td>NaT</td>
      <td>LARCENY-THEFT</td>
      <td>18800 N 34TH LN</td>
      <td>85027.0</td>
      <td>UNKNOWN</td>
      <td>2019.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>204487</th>
      <td>201900000024064</td>
      <td>2019-01-01</td>
      <td>2019-01-05 08:45:00</td>
      <td>BURGLARY</td>
      <td>9400 N 38TH AVE</td>
      <td>85051.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2019.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>204488</th>
      <td>201900000928388</td>
      <td>2019-01-01</td>
      <td>NaT</td>
      <td>RAPE</td>
      <td>6800 W DEVONSHIRE AVE</td>
      <td>85033.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2019.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>204489</th>
      <td>201900000748401</td>
      <td>2019-01-01</td>
      <td>2019-04-29 10:00:00</td>
      <td>RAPE</td>
      <td>1500 W COLTER ST</td>
      <td>85015.0</td>
      <td>GROUP HOME</td>
      <td>2019.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>204490</th>
      <td>201900000824801</td>
      <td>2019-01-01</td>
      <td>2019-03-06 00:00:00</td>
      <td>RAPE</td>
      <td>1800 E VAN BUREN ST</td>
      <td>85006.0</td>
      <td>HOSPITAL</td>
      <td>2019.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
crime2019.shape[0]
```




    47327




```python
crime2019.zip = crime2019.zip.astype(str)
crime2019["Address"] = crime2019["100_block_addr"] +","+crime2019["zip"].str[:-2]
crime2019["Address"].head()
```

    /usr/local/lib/python3.6/dist-packages/pandas/core/generic.py:5208: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      self[name] = value
    /usr/local/lib/python3.6/dist-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      
    




    204486          18800 N 34TH LN,85027
    204487          9400 N 38TH AVE,85051
    204488    6800 W DEVONSHIRE AVE,85033
    204489         1500 W COLTER ST,85015
    204490      1800 E VAN BUREN ST,85006
    Name: Address, dtype: object



### Geopy will not serve 47k request in a single day so we will get Latitude and Longitude of top 100 crimes that happened only in January.


```python
crime_jan_2019_top100 = crime2019[crime2019['Month'] == 1.0].head(100)
```


```python
crime_jan_2019_top100.head()
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
      <th>inc_number</th>
      <th>occurred_on</th>
      <th>occurred_to</th>
      <th>ucr_crime_category</th>
      <th>100_block_addr</th>
      <th>zip</th>
      <th>premise_type</th>
      <th>Year</th>
      <th>Month</th>
      <th>Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>204486</th>
      <td>201900000855221</td>
      <td>2019-01-01</td>
      <td>NaT</td>
      <td>LARCENY-THEFT</td>
      <td>18800 N 34TH LN</td>
      <td>85027.0</td>
      <td>UNKNOWN</td>
      <td>2019.0</td>
      <td>1.0</td>
      <td>18800 N 34TH LN,85027</td>
    </tr>
    <tr>
      <th>204487</th>
      <td>201900000024064</td>
      <td>2019-01-01</td>
      <td>2019-01-05 08:45:00</td>
      <td>BURGLARY</td>
      <td>9400 N 38TH AVE</td>
      <td>85051.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2019.0</td>
      <td>1.0</td>
      <td>9400 N 38TH AVE,85051</td>
    </tr>
    <tr>
      <th>204488</th>
      <td>201900000928388</td>
      <td>2019-01-01</td>
      <td>NaT</td>
      <td>RAPE</td>
      <td>6800 W DEVONSHIRE AVE</td>
      <td>85033.0</td>
      <td>SINGLE FAMILY HOUSE</td>
      <td>2019.0</td>
      <td>1.0</td>
      <td>6800 W DEVONSHIRE AVE,85033</td>
    </tr>
    <tr>
      <th>204489</th>
      <td>201900000748401</td>
      <td>2019-01-01</td>
      <td>2019-04-29 10:00:00</td>
      <td>RAPE</td>
      <td>1500 W COLTER ST</td>
      <td>85015.0</td>
      <td>GROUP HOME</td>
      <td>2019.0</td>
      <td>1.0</td>
      <td>1500 W COLTER ST,85015</td>
    </tr>
    <tr>
      <th>204490</th>
      <td>201900000824801</td>
      <td>2019-01-01</td>
      <td>2019-03-06 00:00:00</td>
      <td>RAPE</td>
      <td>1800 E VAN BUREN ST</td>
      <td>85006.0</td>
      <td>HOSPITAL</td>
      <td>2019.0</td>
      <td>1.0</td>
      <td>1800 E VAN BUREN ST,85006</td>
    </tr>
  </tbody>
</table>
</div>




```python
geolocator = Nominatim(user_agent = "explorer")
  
from geopy.exc import GeocoderTimedOut

def do_geocode(addr):
    try:
      
      location = geolocator.geocode(addr)
        
    except GeocoderTimedOut:
      
      return do_geocode(addr)
    finally:
      if location:
        Lat = location.latitude
        Long = location.longitude
        return Lat,Long
      else:
        return "None","None"
      
crime_jan_2019_top100["Lat_Long"] = crime_jan_2019_top100["Address"].apply(do_geocode)
```


```python
crime_jan_2019_top100["Lat_Long"].head()
```




    204486                     (33.657187, -112.133905)
    204487                       (33.57225, -112.13921)
    204488                     (33.497029, -112.206357)
    204489                     (33.513201, -112.091343)
    204490    (33.452743850000004, -112.04229479741876)
    Name: Lat_Long, dtype: object




```python
len(crime_jan_2019_top100[crime_jan_2019_top100["Lat_Long"] == ('None','None')])
```




    6



- It has 6 rows whose address geopy module couldn't decode so we will not select those and save the remaining to the csv file.


```python
toph_clean = crime_jan_2019_top100[crime_jan_2019_top100["Lat_Long"] != '("None","None")']
```


```python
toph_clean.to_csv("pheonix2019_top100.csv")
```
