# Capstone Project - The Battle of Neighborhoods
This file is the final report I created for the [Applied Data Science Capstone](https://www.coursera.org/learn/applied-data-science-capstone) course, part of the IBM [Applied Data Science Specialization](https://www.coursera.org/specializations/applied-data-science) and the [Data Science Professional Certificate](https://www.coursera.org/specializations/ibm-data-science-professional-certificate).

# Section 1: Introduction
Many large cities in Canada, including those in the Greater Toronto Area, have been experiencing significant increases in real estate prices. This has caused a number of social issues, such as migrations of younger people to areas where the housing costs are more affordable.

The purpose of this project is to further develop the concepts initially explored in this course. More specifically, I intend to further leverage the exploration of Toronto neighborhoods initially done using FourSquare data, and add data about house prices, to understand whether there is a relationship between those and the types of venues near the properties being sold.

This way, when listing properties, both owners and real estate agents can more accurately predict their value based on the types of venues available (or not) nearby.

## Target Audience
The target audience for this project are real estate agents in the Toronto area, as well as anyone planning on buying a house or investing in real estate in the area.

# Section 2: Data
## Ontario Property Sales
The primary data set I will use for this project is the "[House Sales in Ontario](https://www.kaggle.com/mnabaee/ontarioproperties)" data set, available from Kaggle under a [Creative Commons CC0:Public Domain](https://creativecommons.org/publicdomain/zero/1.0/) license.
This data set has the following features:
* (unnamed): index
* Address: Street address of the property in question
* AreaName: Neighborhood where the property is located
* Price ($): Selling price of the property
* lat: Latitude
* lng: Longitude

Here is a sample of a few rows of data:

|(blank)|Address|AreaName|Price ($)|lat|lng|
|---|---|---|---:|---:|---:|
|0|86 Waterford Dr Toronto, ON|Richview|999888|43.679882|-79.544266|
|1|#80 - 100 BEDDOE DR Hamilton, ON|Chedoke Park B|399900|43.25|-79.904396|
|2|213 Bowman Street Hamilton, ON|Ainslie Wood East|479000|43.25169|-79.919357|
|3|102 NEIL Avenue Hamilton, ON|Greenford|285900|43.227161|-79.767403|
|8|532 Caledonia Rd Toronto, ON|Fairbank|25|43.691193|-79.461662|

As it can be seen above, there are a few oddities in the data: the indexes skip some numbers, and some of the prices don't
 make sense. These will be further explored and addressed later in the project (see the 'Methodology' section).
 
## FourSquare Venue Data
The second data set I used was the FourSquare venue data, obtained in JSON format by using the FourSquare API with a Sandbox Developer account. Here is a sample of this data:

```json
{'meta': {'code': 200, 'requestId': '5bcce63c6a6071462c4a37f0'},
 'response': {'groups': [{'items': [{'reasons': {'count': 0,
       'items': [{'reasonName': 'globalInteractionReason',
         'summary': 'This spot is popular',
         'type': 'general'}]},
      'referralId': 'e-0-4ff1dbf1e4b07cca845d6e91-0',
      'venue': {'categories': [{'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/food/juicebar_',
          'suffix': '.png'},
         'id': '52f2ab2ebcbc57f1066b8b41',
         'name': 'Smoothie Shop',
         'pluralName': 'Smoothie Shops',
         'primary': True,
         'shortName': 'Smoothie Shop'}],
       'id': '4ff1dbf1e4b07cca845d6e91',
       'location': {'address': '265 Wincott Drive, Unit 2A',
        'cc': 'CA',
        'city': 'Etobicoke',
        'country': 'Canada',
        'distance': 56,
        'formattedAddress': ['265 Wincott Drive, Unit 2A',
         'Etobicoke ON M9R 2R7',
         'Canada'],
        'labeledLatLngs': [{'label': 'display',
          'lat': 43.67952707,
          'lng': -79.54477308}],
        'lat': 43.67952707,
        'lng': -79.54477308,
        'postalCode': 'M9R 2R7',
        'state': 'ON'},
       'name': 'Booster Juice',
       'photos': {'count': 0, 'groups': []}}},
     {'reasons': {'count': 0,
       'items': [{'reasonName': 'globalInteractionReason',
         'summary': 'This spot is popular',
         'type': 'general'}]},
      'referralId': 'e-0-4dbc7c1543a1d8504b8dbb6c-1',
      'venue': {'categories': [{'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/shops/pharmacy_',
          'suffix': '.png'},
         'id': '4bf58dd8d48988d10f951735',
         'name': 'Pharmacy',
         'pluralName': 'Pharmacies',
         'primary': True,
         'shortName': 'Pharmacy'}],
       'id': '4dbc7c1543a1d8504b8dbb6c',
       'location': {'address': '250 Wincott Dr',
        'cc': 'CA',
        'city': 'Weston',
        'country': 'Canada',
        'distance': 160,
        'formattedAddress': ['250 Wincott Dr', 'Weston ON M9R 2R5', 'Canada'],
        'labeledLatLngs': [{'label': 'display',
          'lat': 43.67946095773931,
          'lng': -79.54616780644683}],
        'lat': 43.67946095773931,
        'lng': -79.54616780644683,
        'postalCode': 'M9R 2R5',
        'state': 'ON'},
       'name': 'Rexall Pharma Plus',
       'photos': {'count': 0, 'groups': []}}},
     {'reasons': {'count': 0,
       'items': [{'reasonName': 'globalInteractionReason',
         'summary': 'This spot is popular',
         'type': 'general'}]},
      'referralId': 'e-0-4cc7946f8c5b236a3a7afa6d-6',
      'venue': {'categories': [{'icon': {'prefix': 'https://ss3.4sqi.net/img/categories_v2/food/pizza_',
          'suffix': '.png'},
         'id': '4bf58dd8d48988d1ca941735',
         'name': 'Pizza Place',
         'pluralName': 'Pizza Places',
         'primary': True,
         'shortName': 'Pizza'}],
       'id': '4cc7946f8c5b236a3a7afa6d',
       'location': {'address': '250 Wincott Dr.',
        'cc': 'CA',
        'city': 'Etobicoke',
        'country': 'Canada',
        'distance': 154,
        'formattedAddress': ['250 Wincott Dr.',
         'Etobicoke ON M9R 2R5',
         'Canada'],
        'labeledLatLngs': [{'label': 'display',
          'lat': 43.67943529490718,
          'lng': -79.54608846049521}],
        'lat': 43.67943529490718,
        'lng': -79.54608846049521,
        'postalCode': 'M9R 2R5',
        'state': 'ON'},
       'name': 'Pizza Pizza',
       'photos': {'count': 0, 'groups': []}}}],
    'name': 'recommended',
    'type': 'Recommended Places'}],
  'headerFullLocation': 'Richview, Toronto',
  'headerLocation': 'Richview',
  'headerLocationGranularity': 'neighborhood',
  'suggestedBounds': {'ne': {'lat': 43.6816820018, 'lng': -79.54178173987276},
   'sw': {'lat': 43.6780819982, 'lng': -79.54675026012723}},
  'suggestedFilters': {'filters': [{'key': 'openNow', 'name': 'Open now'}],
   'header': 'Tap to show:'},
  'totalResults': 7}}
```
From this data, I used the latitude and longitude features to display the data on a map, and the distance and category features to understand how they influence house prices.

## Methodology
The following phases were used in the analysis stage of this project:
1. Data Understanding
2. Data Preparation
3. Modelling
4. Evaluation

The first step was to understand the data. This was done by examining the Ontario Property Sales data using Excel, by confirming the data types were appropriate using the dataframe's `.dtypes` property, and by using the `describe()` method. A number of data quality issues were found, which will be discussed shortly.

The FourSquare sample `json` data was also examined, but no issues were found.

#### Issues with the Ontario Property Sales Data
1. Some entries are missing the area name.
2. Some entries have zero as the sale price. Were those properties given away for free?
3. Some entries seem to have latitude and/or longitude set to -999. That's invalid.
4. Still looking at latitudes and longitudes, it seems the data set is bigger than just Toronto. Longitude = 1.074519 is not even in North America!
5. Looks like there are some duplicate entries in the data.

### Data Preparation
To prepare the data for modelling, the issues above would need to be addressed. This was the approach used for each:
1. Missing area names: I chose to do nothing about that one, as it was not expected to impact the model
2. Zero sale price: to determine the scope of the problem, I initially used a histogram, but that was not very helpful due to the significant range (difference between maximum and minimum values) of the data. My next step was to use a distribution plot, excluding the top 20% of the data to better see the lower end of the range. I also did line plots and violin plots to confirm (all those visualizations can be seen in my notebook [here](https://github.com/BernardF1/CourseraCapstoneAppliedDS/blob/master/Applied%20Capstone%20Project%20-%20Final.ipynb)).This helped me see there was a significant number of records with suspiciously low values. My theory is that those sale values were entered in thousands of dollars (e.g. 60 instead of 60,000), but, since I did not have a way of confirming this theory, I opted to simply exclude the bottom 1000 sale prices from the data set. A similar violin plot showed some suspiciously high values in that field, so I excluded to the top 1000 as well, eliminating these outliers.
3. Invalid latitudes and longitudes (-999): geolocation data is a key feature for the business problem and to generate the other features necessary for the model. Since the number of observations affected was relatively small (153 out of 20,000+), I visually inspected them. None appeared to be from Toronto, so simply dropped those records.
4. Properties outside Toronto: in the summary statistics of the data, I noticed the range of latitudes and longitudes was too big to represent just the city of Toronto and, in fact, the set represents the entire province of Ontario. In addition, some longitudes seemed invalid, pointing to locations in Europe. To address this, I used `Nominatim()` (part of the `geopy` library) to generate the coordinate for Toronto, and `vincenty()` (also part of `geopy`) to add a feature to the data set representing the distance of any given property to that coordinate, in Kilometres. Upon consulting [Wikipedia](https://en.wikipedia.org/wiki/Toronto), I decided to use a radius of 20 Km as a cutoff to determine whether or not a property is located in Toronto, and eliminated from consideration all properties further away than that. This reduced the data set to just over 4,000 observations.
5. Duplicate observations: I used `.dropna()` to eliminate these observations.

The next step on the data preparation was to use the FourSquare API to find venues close to the properties in question. I chose to define "close" as "within 200 metres". This definition was based on what I consider a comfortable walking distance. I decided to reuse the `getNearbyVenues()` from a previous project for this purpose.

However, due to constraints in my FourSquare account (maximum daily allowance of 950 regular API calls), the data set was too big to address in its entirety. In other words, sampling was required. I used the `sample()` function from the `random` library to ensure the sampling was random. As I was fairly confident that I would not need to test this portion of the code many times in a day, I chose 200 as the sample size. I also visually verified the distribution of properties on the map of Toronto, using the `Map()` funtion of the `folium` library.

I then collected the FourSquare data, which included a list of over 6,000 venues in over 300 categories within 200 m of the sample properties. A couple of properties did not have any nearby venues, so I removed those from the sample set.

The final step in data preparation was to change the text features to numeric values. I used `get_dummies()` from `pandas` for that purpose, then I grouped the observations for each address using the mean value of each feature. Finally, I merged the resulting dataframe with the original set of observations to have a complete feature set.

### Modelling
Since the goal was to predict the property selling price, I recognized it as a regression problem, and since the goal was to use a combination of features as predictors, I determined it was a multivariate regression. I then attempted to fit the Linear Regression algorithm to the data.

I split the sample data in training and test sets using `train_test_split` from the `sklearn.model_selection` library, and used them to fit the Linear Regression model. Unfortunately, using `cross_val_score` from the same library revealed that the model was not good at all at predicting house prices. I attempted further tuning, but it ultimately proved futile.

Still, with the intent of perhaps facilitating future studies, I used Permutation Importance to identify which features most affected the model (still recognizing the model was not good at predicting prices). I identified the top 15 features, out of over 300, that had the biggest impact in the model's predictions, and used them to refit the model. Additional cross-validation showed that the accuracy improved, but only marginally.

Finally, I used Shapley Additive Explanation (SHAP, in short) values to understand how those 15 features were affecting the model, and whether the effect was positive or negative. A SHAP summary plot was produced for ease of interpretation.

## Results
Unfortunately, this study was unable to find a method to predict property prices in Toronto solely based on the nearby venues. While the sample size was limited based on the FourSquare account restrictions, I don't believe a larger number of observations would have yielded substantially better results.

## Discussion
I believe the data set used was not complete enough to predict property values. Providing additional features, such as property size, should result in a much more accurate model. In addition, using the Area Name feature would probably increase the accuracy of predictions substantially, but that was not the goal of this study. I would recommend future studies to incorporate these recommendations, and maybe use the features I identified as being most important to the model, in conjunction with the additional features, to understand the sensitivity (positive or negative) of property prices to the proximity of certain venues.
