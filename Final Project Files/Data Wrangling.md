**Capstone Data Wrangling**

**Datasets used:**

-   Chicago crime reports from 2001 into 2018 provided by the City of Chicago
    (<https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2>)

-   Chicago train ('L') stops provided by the City of Chicago
    (<https://data.cityofchicago.org/Transportation/CTA-System-Information-List-of-L-Stops/8pix-ypme>)

-   Chicago bus stops provided by the City of Chicago
    (<https://data.cityofchicago.org/Transportation/CTA-Bus-Stops-kml/84eu-buny>)

-   Chicago business licenses provided by the City of Chicago
    (<https://data.cityofchicago.org/Community-Economic-Development/Business-Licenses/r5kz-chrr>)

-   Chicago police stations provided by the City of Chicago
    (<https://data.cityofchicago.org/Public-Safety/Chicago-Police-Department-Illinois-Uniform-Crime-R/c7ck-438e>)

-   U.S. federal holidays provided by Kaggle
    (<https://www.kaggle.com/gsnehaa21/federal-holidays-usa-19662020>)

All of the above datasets, with the exception of the bus stop data, were csv
files and were imported directly as Pandas data frames. The bus stops were
provided in kmz format. I used MyGeodata Converter
(<https://mygeodata.cloud/converter/>) to convert this file to csv format and
then imported it as a Pandas data frame.

**Chicago Crime Reports**

This dataset consisted of 22 columns and 6,726,718 rows with each row being a
reported crime. The columns that were used during the data cleaning are as
follows:

-   ID:

    -   There is a unique identifier for each report.

-   Case Number:

    -   This is the Chicago Police Records Division Number, which is unique to
        the incident.

-   Date:

    -   This is the approximate date and time when the incident occurred.

-   Block:

    -   This is the block where the incident occurred. It is in the format of a
        partially obscured street address.

-   Primary Type:

    -   This is the primary description of the IUCR code. This is what I will be
        trying to predict.

-   Location Description:

    -   This is the description of the location where the incident occurred.

-   Beat:

    -   This is the beat where the incident occurred. A beat is the smallest
        police geographic area.

-   District:

    -   This is the police district where the incident occurred.

-   Ward:

    -   This is the ward where the incident occurred.

-   Community Area:

    -   This is the community where the incident occurred.

-   X Coordinate:

    -   This is the x coordinate of the location where the incident occurred. It
        is shifted from the actual location, but falls on the same block.

-   Y Coordinate:

    -   This is the y coordinate of the location where the incident occurred. It
        is shifted from the actual location, but falls on the same block.

-   Year:

    -   This is the year the incident occurred.

-   Latitude:

    -   This is the latitude of the location where the incident occurred. It is
        shifted from the actual location, but falls on the same block.

-   Longitude:

    -   This is the longitude of the location where the incident occurred. It is
        shifted from the actual location, but falls on the same block.

**ID:**

The ‘ID’ column was kept as it would be useful to have a unique identifier for
each report in case I had to split and merge my data. I verified that there was
a unique ID for each report by finding the length of the set of the ‘ID’ column.
It was the same as the number of rows.

I then dropped the duplicate reports while excluding the ‘ID’ column using
drop_duplicates. This removed 206 reports.

**Case Number:**

I used the case number to check for any more duplicate reports. There were 175
case numbers that had more than one report. I looked up a few of the cases
online and saw that in general, duplicate case numbers meant there were multiple
victims in the same location on the same day. There was one case I saw where the
same case number was used for a different day but the same location. No further
information was found online regarding this incident, so it is unknown if this
was a case of a case number being mismarked or if the incidents were connected.
I therefore left the reports with duplicate case numbers in the dataset.

**Date:**

After performing dropna() on the ‘Date’ column, there were no apparent null. I
created a regular expression matching the syntax of the date and checked that
all the dates followed that format. There were no irregular dates. Using
pandas.to_datetime, I converted the date to a datetime object.

**Year:**

After performing dropna() on the ‘Year’ column and checking the length
afterwards, there were no apparent null values. The column was properly imported
as an integer type, so it appeared it was clean.

**Block:**

After performing dropna() on the ‘Block’ column, there were no apparent null
values. I capitalized all of the blocks and then created a regular expression
matching the syntax of the blocks and checked that all blocks followed that
format. There were 2 entries that were listed as ‘XX UNKNOWN’. In both cases the
latitude and longitude were missing. I left these reports in the dataset as I
didn’t know exactly which variables would be used in my model. Several blocks
showed up with single quotes, accents, and random characters. To simplify this
column, all of these characters were stripped.

**Primary Type:**

After performing dropna() on the ‘Primary Type’ column, there were no apparent
null values. I capitalized all of the types of crime and performed
value_counts() to get the unique values. I saw that there were 3 times that
‘NON-CRIMINAL’ showed up: ‘NON-CRIMINAL’, ‘NON - CRIMINAL’, and ‘NON-CRIMINAL’
(SUBJECT SPECIFIED). I removed the spaces around the hyphen and the ‘(SUBJECT
SPECIFIED)’.

**Location Description**

After performing dropna() on the ‘Location Description’ column, there were 3974
null values found. One way I used to figure out the location description was to
use the ‘Domestic’ column which said if the crime was domestic related. Using
value_counts() for reports which were domestic related, I found that domestic
related crimes occurred more in residences. For reports that were domestic
related, I therefore filled in ‘RESIDENCE’ for the missing location description.
Unfortunately, this only removed 1 null value and there were no other features I
could use to accurately estimate the location description. I left the remaining
reports with the missing location descriptions in the dataset as I didn’t know
exactly which variables will be used in my model. To further clean this column,
I capitalized all entries and removed extra spaces around hyphens and
backslashes.

**Beat:**

After performing dropna() on the ‘Beat’ column, there were no apparent null
values. This column was properly imported as an integer type, so it appeared it
was clean.

**District:**

After performing dropna() on the ‘District’ column, there were 47 null values
found. Looking at the value counts, there were very low report counts for
districts 21 and 31. A look
at <https://home.chicagopolice.org/community/districts/> showed that these
districts no longer existed. I therefore made these districts null.

Examining the police beat boundaries
(<https://data.cityofchicago.org/Public-Safety/Boundaries-Police-Beats-current-/aerh-rz74>)
and police district boundaries
(<https://data.cityofchicago.org/Public-Safety/Boundaries-Police-Districts-current-/fthy-xz3r)>,
it appeared that the police beats generally fell within the boundaries of the
police districts. In order to find the missing districts, I created a dictionary
with beat as the key and district as a value. I then used the dictionary to fill
in the missing districts.

**Ward:**

After performing dropna() on the ‘Ward’ column, there were 614,846 null values
found. Examining the police beat boundaries
(<https://data.cityofchicago.org/Public-Safety/Boundaries-Police-Beats-current-/aerh-rz74>)
and ward boundaries
(<https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-Wards-2015-/sp34-6z76)>,
it appeared that the police beats did not fit nicely within the wards. So the
technique used for the police district could not be accurately used here. Likely
the latitude/longitude would have to be used to figure out the ward. I left
these reports as is and will revisit this if it turns out that ward is a
valuable variable in my model.

**Community Area:**

After performing dropna() on the ‘Community Area’ column, there were 616,022
null values found. Examining the police beat boundaries
(<https://data.cityofchicago.org/Public-Safety/Boundaries-Police-Beats-current-/aerh-rz74>)
and community boundaries
(<https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-Community-Areas-current-/cauq-8yn6)>,
it appeared that the police beats did not fit nicely within the communities. So
the technique used for the police district could not be accurately used here.
Likely the latitude/longitude would have to be used to figure out the community.
I left these reports as is and will revisit this if it turns out that community
is a valuable variable in my model.

Looking at the value counts for each community, there were 91 reports for
community 0. There is no community 0, so I changed this community to null.

**Latitude/Longitude**:

After performing dropna(), there were 60,175 null values found for latitude and
longitude. I created a regular expression matching the syntax of the
latitude/longitude and checked that all entries followed that format. No
irregular locations were found this way. I then plotted the latitude and
longitude and found points well southwest of the Chicago Area (shown below).

![](media/55d367555a816d48c4c17bab2b2ef83b.png)

After examining some of these points, it appeared that the latitude/longitude of
36.619446/-91.686566 was given to several crime reports. It may be the case that
this position was used when the latitude/longitude was not noted. I made these
positions null values and will revisit them later. In total, there were 60,336
reports missing latitude/longitude.

**X Coordinate/Y Coordinate:**

After performing dropna() there were 60,175 null values found for the
coordinates. I created a regular expression matching the syntax of the
coordinates and checked that all entries followed that format. No irregular
coordinates were found this way. I then plotted the X coordinate and Y
coordinate and again found points well southwest of the Chicago Area (shown
below).

![](media/4e5c1e6aeea4f744e1b2595100c0be25.png)

It appeared that when the latitude/longitude were not known, the X/Y coordinates
were 0. I made these null values and will revisit them later. In total, there
were 60,336 reports with missing X/Y coordinates.

**Adding Columns:**

Using the ‘Date’ column, columns for the month, day of the month, day of the
week, day type (weekday/weekend), and hour were found. Using month, a column for
seasons was created with winter (December, January, February), spring (March,
April, May), summer (June, July, August), and fall (September, October,
November). Using month, a column for quarter of the year was also created with
Q1 (January, February, March), Q2 (April, May, June), Q3 (July, August,
September), Q4 (October, November, December). Using the day of the month, a
column for the third of the month was created with T1 (days 1-10), T2 (days
11-20), and T3 (days 21-31). Using the hour, a column for time of day was
created with overnight (hour 0-5), morning (hour 6-11), afternoon (hour 12-17),
and evening (hour 18-23).

Using the ‘Block’ column, a column for street was created by splitting up the
block into 2 pieces and taking the second piece with the direction and street
name.

I used the following coordinates for the Chicago city center: 41.881832,
-87.623177. These coordinates are from
<https://www.latlong.net/place/chicago-il-usa-1855.html>. Using the haversine
formula, I created a new column with the distance between each crime report and
the Chicago city center.

**Bus Stops**

**Cleaning:**

This dataset consisted of 21 columns and 10,916 rows with each row being a bus
stop. There were no apparent null values for latitude/longitude and the bus stop
name. There was a ‘Status’ column which showed if the bus stop was still in
service. There were several stops that were flagged, but an online search of a
few of these stops showed that they were still in service. I therefore kept all
of the bus stops. I then plotted all of the locations of the bus stops (shown
below).

![](media/6d19527536e7b2266ae7ae7122cd3835.png)

There were a couple of far western bus stops west of -87.85. Further
investigation showed that they were valid UPS stops.

**Distance from Closest Bus Stop:**

I used a ball tree query to find the closest bus stop and the distance from the
closest bus stop for each crime report. First I converted the bus stop
latitude/longitudes to radians and zipped them together. Then I created a ball
tree using these coordinates. I converted the crime report latitude/longitudes
to radians and zipped them together and performed a query on the ball tree.

**Train Stops**

**Cleaning:**

This dataset consisted of 17 columns and 300 rows with each row being a train
stop. There were no apparent null values for the station names and locations.
There was a column with a descriptive name of the station which included the
train line in parentheses. I extracted the train line using .split and created a
new column called ‘Line’. The location column contained a tuple of the latitude
and longitude. I extracted the latitude and longitude into separate columns
using .split. I then plotted all of the locations of the train stops (shown
below) and all looked good.

![](media/26f55c491baa3e36853fc42cabdca85c.png)

**Distance from Closest Train Stop:**

I used a ball tree query to find the closest train stop, the associated train
line, and the distance from the closest train stop for each crime report. First
I converted the train stop latitude/longitudes to radians and zipped them
together. Then I created a ball tree using these coordinates. I converted the
crime report latitude/longitudes to radians and zipped them together and
performed a query on the ball tree.

**Liquor Stores**

**Cleaning:**

There were 34 columns and 954,489 rows with each row being a business license.
The columns I used were the legal name, latitude/longitude, and address. There
was a column that showed the license status, but I decided to look at all of the
businesses as they were likely operating at some point during the period of
2001-2018.

There were 4 businesses with missing legal names. Fortunately, these businesses
had nothing to do with liquor, so I removed them. In order to find liquor
stores, I searched for businesses that contained one of the following terms:
liquor, spirits, wine, or alcohol. I sorted the resulting data frame by legal
name and saw that there were businesses that were duplicates as they had to
regularly apply for licenses. They were dropped using drop_duplicates().

There were 4 liquor stores that did not have a latitude/longitude. I used the
website <https://www.latlong.net/convert-address-to-lat-long.html> to convert
their addresses to latitude/longitude. One store was located outside of the
Chicago Area so I deleted it. I then plotted all of the locations of the liquor
stores (shown below) and all looked good.

![](media/07176146990f18f97e89ea454686248e.png)

**Distance from Closest Liquor Store:**

I used a ball tree query to find the closest liquor store and the distance from
the closest liquor store for each crime report. First I converted the liquor
store latitude/longitudes to radians and zipped them together. Then I created a
ball tree using these coordinates. I converted the crime report
latitude/longitudes to radians and zipped them together and performed a query on
the ball tree.

**Police Stations**

**Cleaning:**

This dataset contained 9 columns and 22 rows with each row being a police
station (district). The columns I used were district and location. The police
headquarters was listed as a district in this dataset. As the headquarters was
not used anywhere in my Chicago crime dataset, I removed the headquarters. The
location column contained a tuple of the latitude and longitude. I split this
column using .split and created new columns for the latitude and longitude. I
then plotted all of the locations of police stations (shown below) and all
looked good.

![](media/42958c7bc0801205c6578b157d5eef4d.png)

**Distance from Closest Police Station:**

I used a ball tree query to find the closest police station and the distance
from the closest police station for each crime report. First I converted the
police station latitude/longitudes to radians and zipped them together. Then I
created a ball tree using these coordinates. I converted the crime report
latitude/longitudes to radians and zipped them together and performed a query on
the ball tree.

**Federal Holidays**

This dataset contained 2 columns and 485 rows with each row being a federal
holiday. The 2 columns were date and holiday. I first created a new dataset with
just the holidays occurring during 2001 through 2018. I then counted how many
holidays occurred each year. All years except for 2010 and 2011 had 10 holidays.
It turned out that 2010 had an extra occurrence of New Year’s and 2011 was
missing New Year’s. I then checked out a list of the unique holidays and saw
that Martin Luther King, Jr. Day, New Year's Day, and Washington's Birthday had
different notations. These issues where fixed using str.replace.

I then combined the dataframe of holidays with the dataframe of crime reports
using .merge and filled the null values in the holiday column with ‘No Holiday’.
I also created a new column called ‘Is Holiday’ which said if a specific day was
a holiday or not.
