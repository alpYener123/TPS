# TPS -- Turkey Positioning System

TPS, detects city-level locations of tweets that were collected via Twitter API v1 or v2

Works for Turkish cities only.

First, gather the necessary files via class GatherFiles. Then, conduct the search. An example can be found below. The necessary data for the GatherFiles object can be found under ```data/```

Population bias 
- Active: If a district name is included in multiple cities, estimate that the tweet is from the city with the larger population. pop_bias parameter is set to ```False``` by default.
- Inactive: Do not do this

Districts searched:
- City name
- "Ilçe" name (smaller part of a city)
- "Semt" name (smaller part of an ilçe)

## Example

```python
from TPS import GatherFiles, GatherLoc
files = GatherFiles()
files.create_district_mappings(path_excel) # the excel which is named city_street.xlsx on data/
city_list = files.write_data_return_list(cities_path=city_list_path, data_path_json=empty_data_path)
# cities_path: a list of cities. Can be found on data/
# data_path_json: just a regular file name, ending with .json. This is going to be written with the function

city_data = files.get_city_data(empty_data_path)
# Gathered the files needed to create GatherLoc object

tps_finder = get_locations(city_data=city_data, data_folder_path=dir_path, which_metadata="user_bio", path_result="v2_trial_tweet-based.json", api_version=2, user=False, retweets=True, path_dates="dates.txt", path_texts=txt_path, search_keyword="@RTErdogan", date_window=[[2023,4,10], 6])
```

### Explanation
```city_parts``` fills up the empty dictionaries of ```GatherFiles``` object. These dictionaries are needed for the creation of a ```GatherLoc``` object
- ```path_excel``` example: ```data/city_street.xlsx```
 <br />

```city_list_and_data``` returns a list of cities. Also saves a json file as all city names as keys and all values defaultly set to 0.
- ```cities_path``` example: ```data/cities.txt```
- ```data_path_json``` a json path which the json file is written onto. example: ```trial_data_gathered/empty_data.json```
<br />

```get_user_loc``` searches through the data and gets the user locations of users that have legitimate location info in ```[user][location]``` part of the metadata
- Arguments:
    - ```city_data``` a dictionary with city names as keys and some integer value as values.
    - ```main_data_path``` path to the main data (that is, data collected via Twitter API v1)
    - ```result_path_JSON``` path which the end result of city_data will be written to. Must be a json file


More functions and their usage examples are on ```trial_data.ipynb```

## Info about the Files on the Repo

```data``` --> Necessary files for the search. Can be updated/altered if needed.

```LocationProject``` --> directory for the package for finding locations of tweets.
- ```trial_data.ipynb``` --> Gathers the data in all possible ways and stores it in ```trial_data_gathered'''```
    -  Accumulation: Using all 3 ways back to back, building up on each other. Hence, we get a total profile of people with accessible location data.
        Order of operation: user bio, tweet bio, tweet coordinate.
    - Individual: Used all 3 methods individually to see how many users had what info.
- ```TweetLocationFinder```
    - ```EntityExtractor``` --> Extracts the location entities on the given sentence via language models. Then, finds (if there is) a corresponding city and displays it.
        - ```GatherUserLoc```
            - ```GatherFiles``` --> gathers the files needed in a usable data type
            - ```GatherLoc``` --> actually gathers the location info via the 3 ways explained above

```graphs``` --> the graphs of the data gathered

```extracting_graphs.ipynb``` --> forming those graphs

## Links Used

```data/city_street.xlsx``` --> https://postakodu.ptt.gov.tr/ <br />

```data/cities.txt``` --> https://engelsizdestek.org/iller <br />

```data/populations.json``` --> https://gist.github.com/ozdemirburak/4821a26db048cc0972c1beee48a408de <br />

```data/turkey.geojson``` --> https://github.com/alpers/Turkey-Maps-GeoJSON <br />

```graphs/explanation/overestimated_cities.png``` --> http://160.75.25.161/index.php/itudergisi_a/article/viewFile/1060/1009
