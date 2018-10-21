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
 make sense. These will be further explored and addressed later in the project.
 
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
