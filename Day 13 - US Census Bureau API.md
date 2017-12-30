<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---us-census-bureau.jpg" width=500>

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/), today I'm writing about the [US Census Bureau APIs](https://www.census.gov/data/developers/data-sets.html), which provide access to American census data, demographics, housing stats, etc. It's all anonymous aggregate data that's already publicly available - no personal data.

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

## Authorization

As usual, you need to [request a key](https://api.census.gov/data/key_signup.html) first. Just enter "none" and your email address, and hopefully you'll receive a success message like this one:

> Your request for a new API key has been successfully submitted. Please check your email. In a few minutes you should receive a message with instructions on how to activate your new key.

I got the email in about 30 seconds, clicked the link to validate the key, and was taken to a *success* page.

> Congratulations! Your key has been activated. You may now use it to query the Data API. Happy querying!

## Geocoding Service

Try your shiny new key with the [geocoding API](https://www.census.gov/data/developers/data-sets/Geocoding-services.html), a service for looking up addresses and getting a latitude/longitude coordinate - similar to the [Google Maps geocoding API](https://grantwinney.com/day-6-google-maps-api/#geocodingapi).

    GET https://geocoding.geo.census.gov/geocoder/locations/onelineaddress?address=50+Public+Square+Cleveland+Ohio&format=json&benchmark=Public_AR_Current&key=<your-key>

I looked up the address for the Terminal Tower in Cleveland OH, and got the address back along with its geolocation coordinates. (Using `x` and `y` as a field name in the response is a poor choice - they represent longitude and latitude respectively, and should've been named likewise.)

```json
{
    "result": {
        "input": {
            "address": {
                "address": "50 Public Square Cleveland Ohio"
            },
            "benchmark": {
                "id": "4",
                "isDefault": false,
                "benchmarkName": "Public_AR_Current",
                "benchmarkDescription": "Public Address Ranges - Current Benchmark"
            }
        },
        "addressMatches": [
            {
                "matchedAddress": "50 PUBLIC SQ, CLEVELAND, OH, 44113",
                "coordinates": {
                    "x": -81.6947,
                    "y": 41.500114
                },
                "tigerLine": {
                    "tigerLineId": "61109032",
                    "side": "R"
                },
                "addressComponents": {
                    "fromAddress": "2",
                    "toAddress": "108",
                    "preQualifier": "",
                    "preDirection": "",
                    "preType": "",
                    "streetName": "PUBLIC",
                    "suffixType": "SQ",
                    "suffixDirection": "",
                    "suffixQualifier": "",
                    "state": "OH",
                    "zip": "44113",
                    "city": "CLEVELAND"
                }
            },
            {
                "matchedAddress": "50 PUBLIC SQ, CLEVELAND, OH, 44141",
                "coordinates": {
                    "x": -81.62801,
                    "y": 41.320698
                },
                "tigerLine": {
                    "tigerLineId": "61154520",
                    "side": "L"
                },
                "addressComponents": {
                    "fromAddress": "2",
                    "toAddress": "98",
                    "preQualifier": "",
                    "preDirection": "",
                    "preType": "",
                    "streetName": "PUBLIC",
                    "suffixType": "SQ",
                    "suffixDirection": "",
                    "suffixQualifier": "",
                    "state": "OH",
                    "zip": "44141",
                    "city": "CLEVELAND"
                }
            }
        ]
    }
}
```

## Language Statistics

I have pretty much no idea what all this census lingo means... it's like greek to me. I'm gonna pick one more API that seems fairly self-explanatory - [language statistics](https://www.census.gov/data/developers/data-sets/language-stats.html). 

For the following calls, you'll need [FIPS codes for states/counties](https://www.census.gov/geo/reference/codes/cou.html) and [language codes](https://www.census.gov/hhes/socdemo/language/about/02_Primary_list.pdf), both available on census.gov. In the text files, the first number (02 or 39 below) is the state code, and 020, 035, etc are the county codes.

    AK,02,013,Aleutians East Borough,H1
    AK,02,016,Aleutians West Census Area,H5
    AK,02,020,Anchorage Municipality,H6
    
    OH,39,033,Crawford County,H1
    OH,39,035,Cuyahoga County,H1
    OH,39,037,Darke County,H1

*The numbers reported by the following calls seem exceptionally low. Ohio has 11.5 million people, but the results say only a quarter million speak Spanish - there's no way it's that few. I'm not sure if it means primary language, or only language, or something else. Additionally, it probably depends on whether residents were even asked, and whether or not they chose to tick the box indicating they spoke Spanish. In other words, I don't think I'd trust this data without knowing more about its limitations.*

### Alaska

Find how many people in Anchorage, AK speak Spanish at home: (12635 people)

    GET https://api.census.gov/data/2013/language?get=NAME,EST,LANLABEL&for=county:020&in=state:02&LAN=625&key=<your-key>

```json
[
    [
        "NAME",
        "EST",
        "LANLABEL",
        "LAN",
        "state",
        "county"
    ],
    [
        "Anchorage Municipality, AK",
        "12635",
        "Spanish",
        "625",
        "02",
        "020"
    ]
]
```

And how many speak French: (1035 people)

    GET https://api.census.gov/data/2013/language?get=NAME,EST,LANLABEL&for=county:020&in=state:02&LAN=620&key=<your-key>

```json
[
    [
        "NAME",
        "EST",
        "LANLABEL",
        "LAN",
        "state",
        "county"
    ],
    [
        "Anchorage Municipality, AK",
        "1035",
        "French",
        "620",
        "02",
        "020"
    ]
]
```

### Ohio

There are 490 Portuguese speakers in Cuyahoga County OH, and 241650 Spanish speakers in Ohio.

    GET https://api.census.gov/data/2013/language?get=NAME,EST,LANLABEL&for=state:39&LAN=625&key=<your-key>

```json
[
    [
        "NAME",
        "EST",
        "LANLABEL",
        "LAN",
        "state"
    ],
    [
        "Ohio",
        "241650",
        "Spanish",
        "625",
        "39"
    ]
]
```

    GET https://api.census.gov/data/2013/language?get=NAME,EST,LANLABEL&for=county:035&in=state:39&LAN=629&key=<your-key>

```json
[
    [
        "NAME",
        "EST",
        "LANLABEL",
        "LAN",
        "state",
        "county"
    ],
    [
        "Cuyahoga County, OH",
        "490",
        "Portuguese",
        "629",
        "39",
        "035"
    ]
]
```

## Thoughts

The website is a little tough on the eyes - at least mine - but the information is all there. Once you pick an API, you can drill down into the documentation and find allowed parameters, sample responses and even sample usages.

The JSON responses are a little weird, and makes it more difficult than necessary to extract a particular field from the results. I'd expect to see the previous results to be returned in a format like this instead, so (for example) querying on "EST" would return "12635".

```json
{
    "NAME": "Anchorage Municipality, AK",
    "EST": "12635",
    "LANLABEL": "Spanish",
    "LAN": "625",
    "state": "02",
    "county": "020"
}
```
