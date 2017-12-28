<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---iss-notify.jpg" width=500>

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/) *([also on GitHub](https://github.com/grantwinney/15-apis-in-15-days))*, today I'm writing about the [ISS Notify API](https://developers.trello.com/v1.0/reference) (or is it the Open Notify API?). Over the past 10 days, I've written about some APIs from [corporations](https://grantwinney.com/day-6-google-maps-api/), and others from [government agencies](https://grantwinney.com/day-8-nasa-api/), but this one is smaller. The author, [Nathan Bergey](https://twitter.com/natronics), wrote it for a competition called Science Hack Day - you can [read more here](http://open-notify.org/about.html).

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

---

## Where is the ISS?

This is great. Could there be an easier way to [find the location of the ISS](http://open-notify.org/Open-Notify-API/ISS-Location-Now/)?

    GET http://api.open-notify.org/iss-now.json

And here's the result:

```json
{
    "timestamp": 1514428436,
    "message": "success",
    "iss_position": {
        "longitude": "63.8579",
        "latitude": "42.9473"
    }
}
```

If you'd like to try it out in a script, here it is in Python2... but you can use it with any language you'd like as long as you know how to parse the JSON response.

```python
import urllib2
import json
import datetime

req = urllib2.Request("http://api.open-notify.org/iss-now.json")
response = urllib2.urlopen(req)
payload = json.loads(response.read())

timestamp = datetime.datetime.fromtimestamp(payload['timestamp'])
date = timestamp.strftime('%Y-%m-%d')
time = timestamp.strftime('%H:%M:%S')
latitude = payload['iss_position']['latitude']
longitude = payload['iss_position']['longitude']

print "On %s at %s, the lat/long of the ISS was: %s %s" % (date, time, latitude, longitude)

# sample output:
# On 2017-12-27 at 22:03:14 the lat/long of the ISS was 4.4077, -174.5883
# On 2017-12-27 at 22:03:20 the lat/long of the ISS was 4.0774, -174.3523
# On 2017-12-27 at 22:04:23 the lat/long of the ISS was 0.8722 , -172.0740
# On 2017-12-27 at 22:04:36 the lat/long of the ISS was 0.2359 -171.6230
# On 2017-12-27 at 22:05:06 the lat/long of the ISS was: -1.2658 -170.5585
# On 2017-12-27 at 22:05:11 the lat/long of the ISS was: -1.5203 -170.3780
```

## When is the ISS overhead?

The other day, I used the [Google Maps API](https://grantwinney.com/day-6-google-maps-api/) to get the location of the Terminal Tower in Cleveland - the lat/long is 41.4984174, -81.6937287 respectively. We can use another endpoint to [calculate when the ISS will fly over](http://open-notify.org/Open-Notify-API/ISS-Pass-Times/) the Terminal Tower (or any other location of your choice).

    GET http://api.open-notify.org/iss-pass.json?lat=41.4984174&lon=-81.6937287

In the response, you get the location you requested back, a unix timestamp, and an array of upcoming locations. (You're supposed to be able to specify the number of passes, but it didn't seem to work.)

```json
{
    "message": "success",
    "request": {
        "altitude": 100,
        "datetime": 1514432297,
        "latitude": 41.4984174,
        "longitude": -81.6937287,
        "passes": 5
    },
    "response": [
        {
            "duration": 489,
            "risetime": 1514455711
        },
        {
            "duration": 642,
            "risetime": 1514461391
        },
        {
            "duration": 600,
            "risetime": 1514467219
        },
        {
            "duration": 562,
            "risetime": 1514473081
        },
        {
            "duration": 613,
            "risetime": 1514478895
        }
    ]
}
```

Again, here's a Python2 script if you'd like to try it out:

```python
import urllib2
import json
import datetime

req = urllib2.Request("http://api.open-notify.org/iss-pass.json?lat=41.4984174&lon=-81.6937287")
response = urllib2.urlopen(req)
payload = json.loads(response.read())

latitude = payload['request']['latitude']
longitude = payload['request']['longitude']
risetime = datetime.datetime.fromtimestamp(payload['response'][0]['risetime'])
duration = payload['response'][0]['duration']

print "The next ISS pass for %s %s is %s for %s seconds" % (latitude, longitude, risetime, duration)

# sample output:
# The next ISS pass for 41.4984174 -81.6937287 is 2017-12-28 05:08:31 for 489 seconds
```

## Who's in Space?

He also threw in an endpoint for getting the names of astronauts currently in space, but his docs say he has to manually update it, so it *may* not always up-to-date... although [it currently is (expedition 54)](https://www.nasa.gov/mission_pages/station/expeditions/expedition54/index.html).

    GET http://api.open-notify.org/astros.json

```json
{
    "number": 6,
    "people": [
        {
            "craft": "ISS",
            "name": "Alexander Misurkin"
        },
        {
            "craft": "ISS",
            "name": "Mark Vande Hei"
        },
        {
            "craft": "ISS",
            "name": "Joe Acaba"
        },
        {
            "craft": "ISS",
            "name": "Anton Shkaplerov"
        },
        {
            "craft": "ISS",
            "name": "Scott Tingle"
        },
        {
            "craft": "ISS",
            "name": "Norishige Kanai"
        }
    ],
    "message": "success"
}
```

Here's one last script, because why not:

```python
import urllib2
import json
import datetime

req = urllib2.Request("http://api.open-notify.org/astros.json")
response = urllib2.urlopen(req)
payload = json.loads(response.read())

print "There are %d people in space:" % (payload['number']),
for i in range(payload['number']):
    print payload['people'][i]['name'] + ",",

# sample output:
# There are 6 people in space: Alexander Misurkin, Mark Vande Hei, Joe Acaba, Anton Shkaplerov, Scott Tingle, Norishige Kanai,

```

## Thoughts

This is a clean API, and really awesome if you want to find and easily consume the location of the ISS for your own purpose, or display a list of upcoming passes on a website or something.

It looks like he opensourced everything too, so if you want to investigate how he did what he did, check out [Open Notify](https://github.com/open-notify) on GitHub. I haven't dug into it yet - I'm not sure if he's collecting raw data periodically, or making calculations and caching the result, or just hitting some other API at NASA and passing the results along. It's cool of him to put in the work and make it publicly accessible though.
