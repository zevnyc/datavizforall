# Geocode Locations into Coordinates with US Census or Google
*By [Ilya Ilyankou and Jack Dougherty](../../introduction/who.md), last updated April 10, 2017*

Many free map tools geocode locations by placing them on a map, such as the [BatchGeo](../../map/batchgeo) and [Google My Maps tutorials](../../map/mymaps) in this book. But those tools typically do not allow you to easily extract the latitude-longitude coordinates for each point.

We created two free Google Sheets Geocoder scripts that have several advantages:
- convert locations (Hartford CT) or addresses (300 Summit St, Hartford CT) into latitude-longitude coordinates (41.748, -72.692) inside your Google Sheet
- show the location found in the geocoding database, and match quality, to review your results
- convert US addresses into US Census geography, such as census tracts, block groups, and blocks

As with any geocoding service, accuracy is not guaranteed. Inspect your results in the Found and Quality columns.

## Google Sheets Geocoder: US Census or Google
- Geocode locations into latitude, longitude, with source and match quality, inside a Google Sheet
- Go to Google Sheet template, sign in to your account, and File > Make a Copy to your Google Drive https://docs.google.com/spreadsheets/d/1XvtkzuVyQ_7Ud47ypDJ4KOmz_5lOpC9sqeEDBbJ5Pbg/edit#gid=0
- Insert locations, select 6 columns, and select Geocoder menu: US Census or Google (limit 1000 daily per user)
- Google Sheets script will ask for permission to run the first time
- Note: The [Leaflet Maps with Google Sheets template](../../leaflet/with-google-sheets) in this book includes this Geocoder script.

![Screencast: Google Sheets Geocoder: US Census or Google](google-sheets-geocoder-census-google.gif)

## Google Sheets Geocoder: US Census Geographies
- Geocode US addresses into latitude, longitude, GeoID, census tract, inside a Google Sheet
- Go to Google Sheet template, sign in to your account, and File > Make a Copy to your Google Drive
https://docs.google.com/spreadsheets/d/1x_E9KwZ88c_kZvhZ13IF7BNwYKTJFxbfDu77sU1vn5w/edit#gid=0
- Insert locations, select 8 columns, and select Geocoder menu: US Census 2010 Geographies
- Google Sheets script will ask for permission to run the first time

![Screencast: GoogleSheets Geocoder: US Census Geographies](google-sheets-geocoder-census-geographies.gif)

#### About US Census 15-character GeoID
- Make sure that column G is formatted as text (to preserve leading zeros), not number
- Break down a sample GeoID: 090035245022001
  - state = 09
  - county = 003
  - tract = 524502 = 5245.02
  - block group = 2
  - block = 001

## How it works
The Google Sheet Geocoder runs from a script insert in the Google Sheet, which calls one of two free geocoding services:
- US Census Geocoder https://geocoding.geo.census.gov/geocoder. See more detailed documentation at http://www.census.gov/geo/maps-data/data/geocoder.html
- Geocode with Google Apps: The Maps Service of Google Apps allows users to geocode street addresses without using the Google Maps API, with a limit of 1,000 searches daily per user, https://developers.google.com/apps-script/reference/maps/geocoder

## How to insert the Geocoder Script into any Google Sheet
If you do not wish to File > Make a Copy of the Google Sheet templates above, you can insert the open-source Geocoder Scripts into your own Google Sheet:
- Go to [Google Sheets Geocoder repo on GitHub](https://github.com/JackDougherty/google-sheets-geocoder)
- Sign in to your Google Sheets, then select Tools > Script Editor
- File > Create New Script File
- Open and copy a script (such as geocoder-census-google.gs) and paste into your Script Editor
- Save and rename to geocoder-census-google.gs)
- Refresh your Google Sheet and look for new Geocoder menu

** TO DO **
- Also describe and link back how to split columns to form multi-columns addresses
- also describe and link back to how to unify columns to form a one-column address
- add this https://developers.google.com/maps/faq#geocoder_queryformat

## See also: Batch upload to US Census
- Available at US Census Geocoder https://geocoding.geo.census.gov/geocoder/
- Upload CSV table with up to 1000 rows for faster processing, in this format, WITHOUT column headers:

| AnyID  | Street | City | State | Zip   |
| :----- | :----- | :--- | :---- | : --- |
| 1      | 300 Summit St  | Hartford | CT | 06106 |

- Find Locations using > Address Batch (returns latitude, longitude coordinates)
- Find Geographies using > Address Batch (returns lat, lng, census geographies)
- Limitations:
  - Inputs and outputs have no column headers, which may confuse novices
  - Large batches may be delayed a few minutes during peak time periods
  - Unmatched addresses need to be manually corrected and re-submitted

### Try it: Batch Upload to US Census
1) Right-click and Save this CSV file to your computer: [sample-addresses-50](https://www.datavizforall.org/transform/geocode/sample-addresses-50.csv). CSV means comma-separated values, a generic spreadsheet format that most data tools can easily open.

2) Use any spreadsheet tool to organize your address data into five columns: any ID number, street, city, state, zip code. **Remove all column headers**.

  ![](address-no-column-headers.png)

Hints:
- If your data lacks ID numbers, quickly [create a column of consecutive numbers](../../transform/calculate/index.html), as shown in this book.
- If your address data includes apartment numbers, leave them in.
- Only the ID and address fields are required. City, state, and zip code may be blank if you lack any of this information, but fewer matches will be exact.
- If your address data is combined into one cell, such as: 300 Summit St, Hartford, CT 06106
  - then try to [clean your data with the split column method](../../clean/spreadsheets) in this book.
- If you need to temporarily move other non-address data columns into a second spreadsheet, remember to paste the column of ID numbers into the second sheet. After geocoding, sort both sheets by the ID column, then paste to rematch the data.

3) Save the file in CSV generic spreadsheet format, in batches of no more than 1,0000 rows per file. Learn more about [saving in CSV format](../../spreadsheet/csv) in this book.

4) Go to US Census Geocoder (https://www.census.gov/geo/maps-data/data/geocoder.html)

5) Select the Find Geographies Using...Address Batch button for maximum results, including lat-long coordinates and census geography (tracts and block groups). *If census geography is not needed, select Find Locations Using...Address Batch.*

6) Click the Choose button to upload your CSV file. Use the default benchmark and vintage settings for the most current data. Click the Get Results button, and be patient if using the service during busy weekday hours.

  ![](census-geocoder-batch.png)

7) Census Geocoder will download the results through your web browser in a file named: GeocodeResults.csv. Since these results do not contain column headers, use the screenshot below for guidance, or [read the Census Geocoder documentation](http://www.census.gov/geo/maps-data/data/geocoder.html) for more details.

  ![](geocode-results.png)

8) Use a spreadsheet tool to open the CSV file. Sort results by the match quality (columns C and D), with these entries: match exact, match non-exact, tie, no-match.

9) For results without an exact match, check the address for typos, and try to re-geocode in a separate CSV file. The US Census Geocoder tool is very good, but not perfect. For a few rows of hard-to-match data, use a different geocoding tool, such as the Google Maps > What's Here feature described at the top of this page, to look up individual addresses and coordinates.

## Learn more
- Aggregate individual rows of data into groups by census area with [pivot tables](../../spreadsheet/pivot).
- [Download census data](../find) by tract or block group, and use the [VLOOKUP formula](../../spreadsheet/vlookup/) to join or merge this rows of data that you have geocoded by census tract or block group.

{% footer %}
{% endfooter %}
