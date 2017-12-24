<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---NOAA.jpg" width="500">

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/), I came across the [NOAA API](https://www.ncdc.noaa.gov/cdo-web/webservices/v2/). [NOAA](http://www.noaa.gov/) is an American agency that studies and charts various conditions in the oceans and atmosphere.

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

---

## Authorization

First things first... we need an access token. NOAA does it a little differently than the other ones I've seen so far. You need to provide an email address and they'll email you a unique token. You can [request an API token](https://www.ncdc.noaa.gov/cdo-web/token) here. I got the email within seconds. 

![noaa-api---request-token-1](https://grantwinney.com/content/images/2017/12/noaa-api---request-token-1.png)

The limits are very generous if you're using it for a small project for yourself or  a team - *"each token will be limited to five requests per second and 10,000 requests per day"*.

For any request you make, include the access token as a header named "token".

![noaa-api---token-header](https://grantwinney.com/content/images/2017/12/noaa-api---token-header.png)

## Requesting Data

Their [web services documentation](https://www.ncdc.noaa.gov/cdo-web/webservices/v2) is pretty straight-forward. It follows a simple format, and each endpoint you can hit is in a tab along the top of that page.

### Get All Stations

Try requesting all stations using:

    GET https://www.ncdc.noaa.gov/cdo-web/api/v2/stations

You'll get the first 25 results. Check out the metadata section that tells you the offset, the number of records returned (limit), and the total records (count). You can adjust the results using "offset" and "limit" parameters.

```json
{
    "metadata": {
        "resultset": {
            "offset": 1,
            "count": 128495,
            "limit": 25
        }
    },
    "results": [
        {
            "elevation": 139,
            "mindate": "1948-01-01",
            "maxdate": "2014-01-01",
            "latitude": 31.5702,
            "name": "ABBEVILLE, AL US",
            "datacoverage": 0.8813,
            "id": "COOP:010008",
            "elevationUnit": "METERS",
            "longitude": -85.2482
        },
        {
            "elevation": 249.3,
            "mindate": "1938-01-01",
            "maxdate": "2015-11-01",
            "latitude": 34.2553,
            "name": "ADDISON, AL US",
            "datacoverage": 0.5059,
            "id": "COOP:010063",
            "elevationUnit": "METERS",
            "longitude": -87.1814
        },
        ...
        ...
    ]
}
````

### Get a Subset of Stations

There are certain filters that can be applied. You can limit stations to a certain geographical location using a FIPS (Federal Information Processing System) code. I don't know if there's a central source for these codes, but [here are some](https://census.gov/geographies/reference-files/2016/demo/popest/2016-fips.html).

Since it's an excel sheet and not everyone can open it, here's a portion of it reproduced:

    09 - Connecticut
    23 - Maine
    25 - Massachusetts
    33 - New Hampshire
    44 - Rhode Island
    50 - Vermont
    34 - New Jersey
    36 - New York
    42 - Pennsylvania
    17 - Illinois
    18 - Indiana
    26 - Michigan
    39 - Ohio
    55 - Wisconsin
    19 - Iowa
    20 - Kansas
    27 - Minnesota
    29 - Missouri
    31 - Nebraska
    38 - North Dakota
    46 - South Dakota
    10 - Delaware
    11 - District of Columbia
    12 - Florida
    13 - Georgia
    24 - Maryland
    37 - North Carolina
    45 - South Carolina
    51 - Virginia
    54 - West Virginia
    01 - Alabama
    21 - Kentucky
    28 - Mississippi
    47 - Tennessee
    05 - Arkansas
    22 - Louisiana
    40 - Oklahoma
    48 - Texas
    04 - Arizona
    08 - Colorado
    16 - Idaho
    30 - Montana
    32 - Nevada
    35 - New Mexico
    49 - Utah
    56 - Wyoming
    02 - Alaska
    06 - California
    15 - Hawaii
    41 - Oregon
    53 - Washington

I made the same call as previously, but used the FIPS code for Maine, limited the result to 5 records, and sorted by oldest `mindate` first:

    GET https://www.ncdc.noaa.gov/cdo-web/api/v2/stations?locationid=FIPS:23&limit=5&sortfield=mindate

Here are the results. Not sure what the dates mean.

```json
{
    "metadata": {
        "resultset": {
            "offset": 1,
            "count": 670,
            "limit": 5
        }
    },
    "results": [
        {
            "elevation": 88.1,
            "mindate": "1885-05-01",
            "maxdate": "1886-08-31",
            "latitude": 43.760911,
            "name": "SEBAGO LAKE, ME US",
            "datacoverage": 0.998,
            "id": "GHCND:USC00177630",
            "elevationUnit": "METERS",
            "longitude": -70.52561
        },
        {
            "elevation": 304.8,
            "mindate": "1885-06-01",
            "maxdate": "1908-01-31",
            "latitude": 45.133333,
            "name": "MAYFIELD, ME US",
            "datacoverage": 0.933,
            "id": "GHCND:USC00175070",
            "elevationUnit": "METERS",
            "longitude": -69.683333
        },
        {
            "elevation": 152.4,
            "mindate": "1885-10-01",
            "maxdate": "1894-02-28",
            "latitude": 44.416667,
            "name": "KENTS HILL, ME US",
            "datacoverage": 0.8617,
            "id": "GHCND:USC00174230",
            "elevationUnit": "METERS",
            "longitude": -70.083333
        },
        {
            "elevation": 27.4,
            "mindate": "1886-02-01",
            "maxdate": "1914-12-31",
            "latitude": 44.583333,
            "name": "FAIRFIELD, ME US",
            "datacoverage": 0.9829,
            "id": "GHCND:USC00172740",
            "elevationUnit": "METERS",
            "longitude": -69.583333
        },
        {
            "elevation": 40.5,
            "mindate": "1886-09-01",
            "maxdate": "2017-10-31",
            "latitude": 44.2202,
            "name": "GARDINER, ME US",
            "datacoverage": 1,
            "id": "GHCND:USC00173046",
            "elevationUnit": "METERS",
            "longitude": -69.789
        }
    ]
}
```

### Get Available Datasets for a Station

Once you've got a list of stations, you can get data for a station. But what set of data do you want? In order to determine that, you'll need to query to see what datasets are available. I selected the last one from the results of the previous query.

    GET https://www.ncdc.noaa.gov/cdo-web/api/v2/datasets?stationid=GHCND:USC00173046

At first glance, it appears I can choose from daily, monthly, and yearly summaries.

```json
{
    "metadata": {
        "resultset": {
            "offset": 1,
            "count": 6,
            "limit": 25
        }
    },
    "results": [
        {
            "uid": "gov.noaa.ncdc:C00861",
            "mindate": "1763-01-01",
            "maxdate": "2017-12-20",
            "name": "Daily Summaries",
            "datacoverage": 1,
            "id": "GHCND"
        },
        {
            "uid": "gov.noaa.ncdc:C00946",
            "mindate": "1763-01-01",
            "maxdate": "2017-11-01",
            "name": "Global Summary of the Month",
            "datacoverage": 1,
            "id": "GSOM"
        },
        {
            "uid": "gov.noaa.ncdc:C00947",
            "mindate": "1763-01-01",
            "maxdate": "2017-01-01",
            "name": "Global Summary of the Year",
            "datacoverage": 1,
            "id": "GSOY"
        },
        {
            "uid": "gov.noaa.ncdc:C00821",
            "mindate": "2010-01-01",
            "maxdate": "2010-01-01",
            "name": "Normals Annual/Seasonal",
            "datacoverage": 1,
            "id": "NORMAL_ANN"
        },
        {
            "uid": "gov.noaa.ncdc:C00823",
            "mindate": "2010-01-01",
            "maxdate": "2010-12-31",
            "name": "Normals Daily",
            "datacoverage": 1,
            "id": "NORMAL_DLY"
        },
        {
            "uid": "gov.noaa.ncdc:C00822",
            "mindate": "2010-01-01",
            "maxdate": "2010-12-01",
            "name": "Normals Monthly",
            "datacoverage": 1,
            "id": "NORMAL_MLY"
        }
    ]
}
```

## Get Data for a Station

You've got a station you're interested in, and the dataset for that station, so all that's left is querying for the actual data. Unfortunately, I have no clue what the returned data *means*.

Oh well, let's get the data first, then try figuring out what it means.

### Get Daily Summary

This request gets the daily summary for the same station we looked up previously. I limited it to the month of January because trying to get the entire year didn't finish after waiting several minutes.

    GET https://www.ncdc.noaa.gov/cdo-web/api/v2/data?stationid=GHCND:USC00173046&datasetid=GHCND&startdate=2017-01-01&enddate=2017-01-31

```json
{
    "metadata": {
        "resultset": {
            "offset": 1,
            "count": 186,
            "limit": 25
        }
    },
    "results": [
        {
            "date": "2017-01-01T00:00:00",
            "datatype": "PRCP",
            "station": "GHCND:USC00173046",
            "attributes": ",,7,0700",
            "value": 61
        },
        {
            "date": "2017-01-01T00:00:00",
            "datatype": "SNOW",
            "station": "GHCND:USC00173046",
            "attributes": ",,7,",
            "value": 76
        },
        {
            "date": "2017-01-01T00:00:00",
            "datatype": "SNWD",
            "station": "GHCND:USC00173046",
            "attributes": ",,7,",
            "value": 279
        },
        {
            "date": "2017-01-01T00:00:00",
            "datatype": "TMAX",
            "station": "GHCND:USC00173046",
            "attributes": ",,7,0700",
            "value": 6
        },
        {
            "date": "2017-01-01T00:00:00",
            "datatype": "TMIN",
            "station": "GHCND:USC00173046",
            "attributes": ",,7,0700",
            "value": -133
        },
        {
            "date": "2017-01-01T00:00:00",
            "datatype": "TOBS",
            "station": "GHCND:USC00173046",
            "attributes": ",,7,0700",
            "value": 6
        },
        ...
        ...
    ]
}
```

The above is just a portion of the results. The "station" is the one I specified, and the "date" falls within the range I specified. The "datatype" is a code that indicates what the record refers to.

I don't feel like going into codes too deeply - you can [find more here](ftp://ftp.ncdc.noaa.gov/pub/data/ghcn/daily/readme.txt), under the header "III. FORMAT OF DATA FILES" - but here are a few to match the results above:

    PRCP = Precipitation (tenths of mm)
   	SNOW = Snowfall (mm)
	SNWD = Snow depth (mm)
    TMAX = Maximum temperature (tenths of degrees C)
    TMIN = Minimum temperature (tenths of degrees C)
    TOBS = Temperature at the time of observation (tenths of degrees C)

I'm guessing that "value" is self-explanatory - like 279 for SNWD is 279mm or about 11" of snow; and 6 for TOBS means .6°C, or about 33°F. The "attributes" though - not sure what those mean.

### Get Yearly Summary

Here's one last one - same station as before, but using the "yearly" dataset filtered to about a 7 year period.

    GET https://www.ncdc.noaa.gov/cdo-web/api/v2/data?stationid=GHCND:USC00173046&datasetid=GSOY&startdate=2010-01-01&enddate=2017-01-31

The results contain different codes than before, but I couldn't find definitions for them this time... so I'm not even sure what to make of this but here it is anyway.

```json
{
    "metadata": {
        "resultset": {
            "offset": 1,
            "count": 214,
            "limit": 25
        }
    },
    "results": [
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "CDSD",
            "station": "GHCND:USC00173046",
            "value": 248.6
        },
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "CLDD",
            "station": "GHCND:USC00173046",
            "attributes": "0",
            "value": 248.6
        },
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "DP01",
            "station": "GHCND:USC00173046",
            "attributes": "0",
            "value": 118
        },
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "DP10",
            "station": "GHCND:USC00173046",
            "attributes": "0",
            "value": 90
        },
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "DP1X",
            "station": "GHCND:USC00173046",
            "attributes": "0",
            "value": 19
        },
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "DSND",
            "station": "GHCND:USC00173046",
            "value": 78
        },
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "DSNW",
            "station": "GHCND:USC00173046",
            "attributes": "0",
            "value": 16
        },
        {
            "date": "2010-01-01T00:00:00",
            "datatype": "DT00",
            "station": "GHCND:USC00173046",
            "attributes": "0",
            "value": 9
        },
        ...
        ...
    ]
}
```

## Thoughts

Emailing an API token, with no apparent way to reset it (maybe there is and I didn't see it), is a little like emailing a password. It wouldn't be my first choice. I wish they did like many other APIs and either let you generate it through the secure website itself, or make a separate API call to return the token.

Each of the endpoints has some examples you can try online without an API token, which is nice for verifying they work, but it's an incredibly limited tool since you can't modify them through the website itself.

I wish there were more detail about what the codes in the results mean. When it got to the point of returning data, there are a lot of codes and numbers that don't seem to be defined at all. Maybe this is only meant to be used by someone with a meteorological background?

I'd love to use this in a project, maybe on the Raspberry Pi. I was thinking maybe query the station closest to my house and have a simple LED light up depending on the weather - green for rain, white for snow, blue for sleet, unlit if no precipitation. It seems like it'd take more research though.
