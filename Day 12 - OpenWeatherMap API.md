<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---openweathermap-api.jpg" width=500>

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/), today I'm writing about the [OpenWeatherMap API](https://openweathermap.org/api), which provides free access to current weather and other weather-related data (uv index, alerts, etc).

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

## Authorization

If you've been following along with [my other API blog posts](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/) at all, then you'll know by now that authorization is nearly always the first step. No exception here.

[Sign up](http://openweathermap.org/appid) to request an API key. You should end up in a user settings area where you can select the "API keys" tab. It showed a message about taking 10 minutes to generate keys but then it already had a key immediately generated and ready to go, so... I don't know.

![openweathermap-api---api-key](https://grantwinney.com/content/images/2017/12/openweathermap-api---api-key.png)

## Get Current Weather

There are a number of ways to [request current weather data](http://openweathermap.org/current) for your area, but the two most accurate ones seem to be using zip code, and using latitude/longitude:

    GET http://api.openweathermap.org/data/2.5/weather?lat=41.4984174&lon=-81.69372869999999&APPID=<your-app-key>
    
    GET http://api.openweathermap.org/data/2.5/weather?zip=44113,US&APPID=<your-app-key>

It returns an abundance of data for the location - you can [read about the result values here](http://openweathermap.org/current#parameter). Here's the result for Cleveland OH, where it's snowing lightly. There's also a block with other current conditions, such as:

* Tempature, in Kelvin (261.02 K is about 10 Fahrenheit)
* Barometric Pressure, in mm (1038 mm is about 40 inches)
* Humidity, percentage (it's currently snowing so the humidity is high)
* Min and Max temps, in Kelvin (deviation for large geographical areas)
* Visibility, in meters (about 9 miles)
* Wind speed and direction, in m/sec (about 6 mph, due South)
* Cloud cover (currently 90%)
* Timestamp of request (which you can [convert to normal time](https://www.epochconverter.com/) here)
* Timestamps of sunset and sunrise (currently 7:52:43 AM and 5:05:01 PM, respectively)

```json
{
    "coord": {
        "lon": -81.69,
        "lat": 41.5
    },
    "weather": [
        {
            "id": 600,
            "main": "Snow",
            "description": "light snow",
            "icon": "13d"
        }
    ],
    "base": "stations",
    "main": {
        "temp": 261.02,
        "pressure": 1038,
        "humidity": 66,
        "temp_min": 259.15,
        "temp_max": 262.15
    },
    "visibility": 14484,
    "wind": {
        "speed": 2.6,
        "deg": 180
    },
    "clouds": {
        "all": 90
    },
    "dt": 1514472900,
    "sys": {
        "type": 1,
        "id": 2166,
        "message": 0.0045,
        "country": "US",
        "sunrise": 1514465563,
        "sunset": 1514498701
    },
    "id": 5150529,
    "name": "Cleveland",
    "cod": 200
}
```

### Finding Latitude/Longitude

If you need to lookup the coordinates of a location, [check out the Google Maps API](https://grantwinney.com/day-6-google-maps-api/) - they have an endpoint for just that purpose. You can parse out the geometry/location values and use those in the weather request.

    GET https://maps.googleapis.com/maps/api/geocode/json?address=50 Public Square Cleveland, Ohio&key=<your-key>

```json
{
    "results": [
        {
            "formatted_address": "50 Public Square, Cleveland, OH 44113, USA",
            "geometry": {
                "location": {
                    "lat": 41.4984174,
                    "lng": -81.69372869999999
                },
            ...
            ...
```

## Get 5-Day Forecast

The process for [getting the 5-day forecast](http://openweathermap.org/forecast5) is pretty much the same as current weather, except you get a lot more data - every 3 hours worth, in fact.

    GET api.openweathermap.org/data/2.5/forecast?lat=41.4984174&lon=-81.69372869999999&APPID=<your-app-key>

Here's a small portion of the results - there's a `dt_txt` field that clearly shows that each "block" of JSON data is for a 3-hour interval.

```json
{
    "cod": "200",
    "message": 0.0044,
    "cnt": 40,
    "list": [
        {
            "dt": 1514570400,
            "main": {
                "temp": 264.96,
                "temp_min": 263.056,
                "temp_max": 264.96,
                "pressure": 1006.4,
                "sea_level": 1041.63,
                "grnd_level": 1006.4,
                "humidity": 100,
                "temp_kf": 1.9
            },
            ...
            "dt_txt": "2017-12-29 18:00:00"
        },
        {
            "dt": 1514581200,
            "main": {
                "temp": 264.29,
                "temp_min": 263.016,
                "temp_max": 264.29,
                "pressure": 1005.31,
                "sea_level": 1040.53,
                "grnd_level": 1005.31,
                "humidity": 100,
                "temp_kf": 1.27
            },
            ...
            "dt_txt": "2017-12-29 21:00:00"
        },
        {
            "dt": 1514592000,
            "main": {
                "temp": 261.97,
                "temp_min": 261.333,
                "temp_max": 261.97,
                "pressure": 1005.29,
                "sea_level": 1040.7,
                "grnd_level": 1005.29,
                "humidity": 100,
                "temp_kf": 0.63
            },
            ...
            "dt_txt": "2017-12-30 00:00:00"
        },
        ...
        ...
```

## Thoughts

It's unclear what the usage limits are. The page where you get an app key warns against sending requests "more than 1 time per 10 minutes from one device/one API key", which seems like an extreme limitation. Yet the page that compares price and packages says "no more than 60 calls per minute" for a free account, and thousands or even hundreds of thousands per minute for paid accounts; that seems more reasonable.

There are several ways to get weather data. It's odd that the method they encourage is to use a "city id" - a value you get from a JSON file they provide, but the file is not organized in any particular order and has well over a million lines in it. They also provide a way that uses city and country, but that's inaccurate - I live by Cleveland, OH but there's also a Cleveland, GA. Guess there could be a use-case, and it's there if you need it, I just don't think I'd use it.

The other APIs available for use with a free account are [Weather Maps](http://openweathermap.org/api/weathermaps), [UV Index](http://openweathermap.org/api/uvi), [Air Pollution](http://openweathermap.org/api/pollution/co), and [Weather Alerts](http://openweathermap.org/triggers). The last three are still in beta, whatever that means - not sure if that means they're not quite reliable yet? Still would be interesting to try. I could see looking up alerts and then having a Raspberry Pi or similar light an LED or sound a siren for certain conditions.
