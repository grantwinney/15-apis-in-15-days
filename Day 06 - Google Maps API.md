<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---Google-Maps-API.jpg" width="500">

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/) *([also on GitHub](https://github.com/grantwinney/15-apis-in-15-days))*, I checked into the [Google Maps API](https://developers.google.com/maps/). Google is a service that... a provider who.... they run the Internet. They have this little mapping service too.

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

---

The Google Maps API isn't so much an API but a [series of APIs](https://developers.google.com/maps/get-started/) - for Android, iOS, web, etc - and each is focused on a small set of tasks. At first this seemed a bit overwhelming, to see a large set of APIs, but each of them is easy to use. I'll pick a few out to play around with. *(does that count as three days then?)*

## Geocoding API

The first one I thought looked interesting was the [Geocoding API](https://developers.google.com/maps/documentation/geocoding/start), which allows you to convert addresses to geolocation coordinates, and vice versa.

### Authorization

Oddly enough, you don't *need* an API key to experiment with these calls - they worked fine for me without it. I'm not sure how they rate limit requests then, but you'd still need one for a real product, so if you're up for it then [request an API key](https://developers.google.com/maps/documentation/geocoding/get-api-key) before doing anything else. Google's docs are pretty straightforward so there's not much else to say, except that you can just select "Create a new project".

![google-maps-geocoding-api---create-project-for-api-key](https://grantwinney.com/content/images/2017/12/google-maps-geocoding-api---create-project-for-api-key.png)

Be sure to copy the key it produces, because I can't find where to view it after the modal dialog closes, other than clicking "Get a key" again. If you figure it out, I'd like to know! As they state, this automatically activates the Google Maps Geocoding API, and generates an unrestricted key.

![google-api---get-api-key](https://grantwinney.com/content/images/2017/12/google-api---get-api-key.png)

### Trying it out

Whether or not you generated a key, you should be able to try a couple things out.

#### Getting coordinates from an address

Try looking up an address to get information about it, including its coordinates.

    GET https://maps.googleapis.com/maps/api/geocode/json?address=50 Public Square Cleveland, Ohio

I decided to lookup the Terminal Tower, a landmark in Cleveland. What I got back included:

* the parsed address, with information I didn't even provide like county and zip
* a nicely, fully formatted string with the address
* the geolocation coordinates

```json
{
    "results": [
        {
            "address_components": [
                {
                    "long_name": "50",
                    "short_name": "50",
                    "types": [
                        "street_number"
                    ]
                },
                {
                    "long_name": "Public Square",
                    "short_name": "Public Square",
                    "types": [
                        "route"
                    ]
                },
                {
                    "long_name": "Downtown",
                    "short_name": "Downtown",
                    "types": [
                        "neighborhood",
                        "political"
                    ]
                },
                {
                    "long_name": "Cleveland",
                    "short_name": "Cleveland",
                    "types": [
                        "locality",
                        "political"
                    ]
                },
                {
                    "long_name": "Cuyahoga County",
                    "short_name": "Cuyahoga County",
                    "types": [
                        "administrative_area_level_2",
                        "political"
                    ]
                },
                {
                    "long_name": "Ohio",
                    "short_name": "OH",
                    "types": [
                        "administrative_area_level_1",
                        "political"
                    ]
                },
                {
                    "long_name": "United States",
                    "short_name": "US",
                    "types": [
                        "country",
                        "political"
                    ]
                },
                {
                    "long_name": "44113",
                    "short_name": "44113",
                    "types": [
                        "postal_code"
                    ]
                }
            ],
            "formatted_address": "50 Public Square, Cleveland, OH 44113, USA",
            "geometry": {
                "location": {
                    "lat": 41.4984174,
                    "lng": -81.69372869999999
                },
                "location_type": "ROOFTOP",
                "viewport": {
                    "northeast": {
                        "lat": 41.4997663802915,
                        "lng": -81.6923797197085
                    },
                    "southwest": {
                        "lat": 41.4970684197085,
                        "lng": -81.6950776802915
                    }
                }
            },
            "place_id": "ChIJI9jXsn_wMIgRRyf46mR97eY",
            "types": [
                "street_address"
            ]
        }
    ],
    "status": "OK"
}
```

#### Getting an address from coordinates

How about trying the reverse, and getting the address again from the coordinates?

    GET https://maps.googleapis.com/maps/api/geocode/json?latlng=41.4984174,-81.69372869999999

I was suprised to find the results (a portion of which is shown below) included far more than I had expected (such as a bus station), only because the coordinates seem _so_ specific.

I searched for my home address and had similar results with the reverse search - the lat/long coords returned my house (cool), an address range on the street that intersects mine (we're a corner lot, so okay), and a few addresses at the county level and the closest major city (weird and vague). It's possible the list that gets returned is most to least specific, but why there are multiple hits at all I don't really get.

```json
{
    "results": [
        {
            "address_components": [
                {
                    "long_name": "50",
                    "short_name": "50",
                    "types": [
                        "street_number"
                    ]
                },
                {
                    "long_name": "Public Square",
                    "short_name": "Public Square",
                    "types": [
                        "route"
                    ]
                },
                {
                    "long_name": "Downtown",
                    "short_name": "Downtown",
                    "types": [
                        "neighborhood",
                        "political"
                    ]
                },
                {
                    "long_name": "Cleveland",
                    "short_name": "Cleveland",
                    "types": [
                        "locality",
                        "political"
                    ]
                },
                {
                    "long_name": "Cuyahoga County",
                    "short_name": "Cuyahoga County",
                    "types": [
                        "administrative_area_level_2",
                        "political"
                    ]
                },
                {
                    "long_name": "Ohio",
                    "short_name": "OH",
                    "types": [
                        "administrative_area_level_1",
                        "political"
                    ]
                },
                {
                    "long_name": "United States",
                    "short_name": "US",
                    "types": [
                        "country",
                        "political"
                    ]
                },
                {
                    "long_name": "44113",
                    "short_name": "44113",
                    "types": [
                        "postal_code"
                    ]
                }
            ],
            "formatted_address": "50 Public Square, Cleveland, OH 44113, USA",
            "geometry": {
                "location": {
                    "lat": 41.4984174,
                    "lng": -81.69372869999999
                },
                "location_type": "ROOFTOP",
                "viewport": {
                    "northeast": {
                        "lat": 41.4997663802915,
                        "lng": -81.6923797197085
                    },
                    "southwest": {
                        "lat": 41.4970684197085,
                        "lng": -81.6950776802915
                    }
                }
            },
            "place_id": "ChIJI9jXsn_wMIgRRyf46mR97eY",
            "types": [
                "street_address"
            ]
        },
        {
            "address_components": [
                {
                    "long_name": "Euclid Av & Ontario St Station",
                    "short_name": "Euclid Av & Ontario St Station",
                    "types": [
                        "bus_station",
                        "establishment",
                        "point_of_interest",
                        "transit_station"
                    ]
                },
                {
                    "long_name": "Downtown",
                    "short_name": "Downtown",
                    "types": [
                        "neighborhood",
                        "political"
                    ]
                },
                {
                    "long_name": "Cleveland",
                    "short_name": "Cleveland",
                    "types": [
                        "locality",
                        "political"
                    ]
                },
                {
                    "long_name": "Cuyahoga County",
                    "short_name": "Cuyahoga County",
                    "types": [
                        "administrative_area_level_2",
                        "political"
                    ]
                },
                {
                    "long_name": "Ohio",
                    "short_name": "OH",
                    "types": [
                        "administrative_area_level_1",
                        "political"
                    ]
                },
                {
                    "long_name": "United States",
                    "short_name": "US",
                    "types": [
                        "country",
                        "political"
                    ]
                },
                {
                    "long_name": "44113",
                    "short_name": "44113",
                    "types": [
                        "postal_code"
                    ]
                }
            ],
            "formatted_address": "Euclid Av & Ontario St Station, Cleveland, OH 44113, USA",
            "geometry": {
                "location": {
                    "lat": 41.498881,
                    "lng": -81.69354199999999
                },
                "location_type": "GEOMETRIC_CENTER",
                "viewport": {
                    "northeast": {
                        "lat": 41.50022998029149,
                        "lng": -81.69219301970848
                    },
                    "southwest": {
                        "lat": 41.4975320197085,
                        "lng": -81.6948909802915
                    }
                }
            },
            "place_id": "ChIJk7Gntn_wMIgR67oq_t6hmaM",
            "types": [
                "bus_station",
                "establishment",
                "point_of_interest",
                "transit_station"
            ]
        },
        ...
        ...
    ],
    "status": "OK"
}
```

## Directions API

That last API was straight-forward. Let's try looking up some directions with the [Directions API](https://developers.google.com/maps/documentation/directions/intro) next. You don't need an API token to test this one either, although certain parameters seem to require it. [Create a new key](https://developers.google.com/maps/documentation/directions/get-api-key) if you want, and append it with `&key=<your-api-key>`.

Pick an origin and destination. I chose the Terminal Tower (again) as the origin, and a Starbucks around the corner from it as the destination (I know, there's probably 2 Starbucks *inside* it), so the results wouldn't be too large. Note the section in the results called "steps", each of which have a start and end, total distance and duration, and even a one-line instruction in English.

There are lots of parameters for specifying how the directions should be calculated. In the following request, I've chosen "walking" as the travel mode, told it to return alternative routes, and set the language to French.

    GET https://maps.googleapis.com/maps/api/directions/json?origin=50+Public+Square+Cleveland,+Ohio&destination=200+Public+Square+130,+Cleveland,+OH+44114&language=fr&mode=walking&alternatives=true

```json
{
    "geocoded_waypoints": [
        {
            "geocoder_status": "OK",
            "place_id": "ChIJI9jXsn_wMIgRRyf46mR97eY",
            "types": [
                "street_address"
            ]
        },
        {
            "geocoder_status": "OK",
            "partial_match": true,
            "place_id": "ChIJL83I43_wMIgRdZlcIZMKIJ4",
            "types": [
                "premise"
            ]
        }
    ],
    "routes": [
        {
            "bounds": {
                "northeast": {
                    "lat": 41.4998051,
                    "lng": -81.6923484
                },
                "southwest": {
                    "lat": 41.4986974,
                    "lng": -81.6937592
                }
            },
            "copyrights": "Données cartographiques ©2017 Google",
            "legs": [
                {
                    "distance": {
                        "text": "0,1 miles",
                        "value": 192
                    },
                    "duration": {
                        "text": "3 minutes",
                        "value": 167
                    },
                    "end_address": "200 Public Square, 200 Public Square, Cleveland, OH 44114, États-Unis",
                    "end_location": {
                        "lat": 41.4998051,
                        "lng": -81.69261209999999
                    },
                    "start_address": "50 Public Square, Cleveland, OH 44113, États-Unis",
                    "start_location": {
                        "lat": 41.4986974,
                        "lng": -81.6937592
                    },
                    "steps": [
                        {
                            "distance": {
                                "text": "0,1 miles",
                                "value": 192
                            },
                            "duration": {
                                "text": "3 minutes",
                                "value": 167
                            },
                            "end_location": {
                                "lat": 41.4998051,
                                "lng": -81.69261209999999
                            },
                            "html_instructions": "Prendre la direction <b>nord-est</b> sur <b>S Roadway</b> vers <b>Ontario St</b><div style=\"font-size:0.9em\">Votre destination se trouvera sur la droite.</div>",
                            "polyline": {
                                "points": "{eh|F~xrqN?AAEAGAEc@eASg@Se@c@gAQa@AACACAAAC?C?C@C@CBk@f@ID"
                            },
                            "start_location": {
                                "lat": 41.4986974,
                                "lng": -81.6937592
                            },
                            "travel_mode": "WALKING"
                        }
                    ],
                    "traffic_speed_entry": [],
                    "via_waypoint": []
                }
            ],
            "overview_polyline": {
                "points": "{eh|F~xrqNEUoB{ESc@GCM?}@r@"
            },
            "summary": "S Roadway",
            "warnings": [
                "Le calcul d'itinéraires piétons est en bêta. Faites attention – Cet itinéraire n'est peut-être pas complètement aménagé pour les piétons."
            ],
            "waypoint_order": []
        },
        {
            "bounds": {
                "northeast": {
                    "lat": 41.4998051,
                    "lng": -81.6923484
                },
                "southwest": {
                    "lat": 41.4986913,
                    "lng": -81.69384509999999
                }
            },
            "copyrights": "Données cartographiques ©2017 Google",
            "legs": [
                {
                    "distance": {
                        "text": "0,1 miles",
                        "value": 221
                    },
                    "duration": {
                        "text": "3 minutes",
                        "value": 177
                    },
                    "end_address": "200 Public Square, 200 Public Square, Cleveland, OH 44114, États-Unis",
                    "end_location": {
                        "lat": 41.4998051,
                        "lng": -81.69261209999999
                    },
                    "start_address": "50 Public Square, Cleveland, OH 44113, États-Unis",
                    "start_location": {
                        "lat": 41.4986974,
                        "lng": -81.6937592
                    },
                    "steps": [
                        {
                            "distance": {
                                "text": "23 pieds",
                                "value": 7
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 5
                            },
                            "end_location": {
                                "lat": 41.49869229999999,
                                "lng": -81.69384509999999
                            },
                            "html_instructions": "Prendre la direction <b>ouest</b> sur <b>S Roadway</b>",
                            "polyline": {
                                "points": "{eh|F~xrqN@B?D?F"
                            },
                            "start_location": {
                                "lat": 41.4986974,
                                "lng": -81.6937592
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "46 pieds",
                                "value": 14
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 20
                            },
                            "end_location": {
                                "lat": 41.4988192,
                                "lng": -81.69381389999999
                            },
                            "html_instructions": "Tourner à <b>droite</b> vers <b>E Roadway</b>",
                            "maneuver": "turn-right",
                            "polyline": {
                                "points": "yeh|FpyrqNYG"
                            },
                            "start_location": {
                                "lat": 41.49869229999999,
                                "lng": -81.69384509999999
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "430 pieds",
                                "value": 131
                            },
                            "duration": {
                                "text": "2 minutes",
                                "value": 93
                            },
                            "end_location": {
                                "lat": 41.4994528,
                                "lng": -81.6925217
                            },
                            "html_instructions": "Tourner à <b>droite</b> vers <b>E Roadway</b>",
                            "maneuver": "turn-right",
                            "polyline": {
                                "points": "sfh|FhyrqN@CAEAIEM[m@MUGQKWKYSq@EMEICGECAA"
                            },
                            "start_location": {
                                "lat": 41.4988192,
                                "lng": -81.69381389999999
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "46 pieds",
                                "value": 14
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 11
                            },
                            "end_location": {
                                "lat": 41.4993763,
                                "lng": -81.6923833
                            },
                            "html_instructions": "Tourner à <b>droite</b> vers <b>E Roadway</b>",
                            "maneuver": "turn-right",
                            "polyline": {
                                "points": "qjh|FfqrqN@EJU"
                            },
                            "start_location": {
                                "lat": 41.4994528,
                                "lng": -81.6925217
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "180 pieds",
                                "value": 55
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 48
                            },
                            "end_location": {
                                "lat": 41.4998051,
                                "lng": -81.69261209999999
                            },
                            "html_instructions": "Prendre <b>à gauche</b> sur <b>E Roadway</b><div style=\"font-size:0.9em\">Votre destination se trouvera sur la droite.</div>",
                            "maneuver": "turn-left",
                            "polyline": {
                                "points": "cjh|FjprqNAACACAAAC?C?C@C@CBk@f@ID"
                            },
                            "start_location": {
                                "lat": 41.4993763,
                                "lng": -81.6923833
                            },
                            "travel_mode": "WALKING"
                        }
                    ],
                    "traffic_speed_entry": [],
                    "via_waypoint": []
                }
            ],
            "overview_polyline": {
                "points": "{eh|F~xrqN@H?FYG?IGWi@cASi@k@cBIK?GJUAAGCM?}@r@"
            },
            "summary": "E Roadway",
            "warnings": [
                "Le calcul d'itinéraires piétons est en bêta. Faites attention – Cet itinéraire n'est peut-être pas complètement aménagé pour les piétons."
            ],
            "waypoint_order": []
        },
        {
            "bounds": {
                "northeast": {
                    "lat": 41.50007040000001,
                    "lng": -81.69261209999999
                },
                "southwest": {
                    "lat": 41.4986913,
                    "lng": -81.69389630000001
                }
            },
            "copyrights": "Données cartographiques ©2017 Google",
            "legs": [
                {
                    "distance": {
                        "text": "0,2 miles",
                        "value": 244
                    },
                    "duration": {
                        "text": "3 minutes",
                        "value": 193
                    },
                    "end_address": "200 Public Square, 200 Public Square, Cleveland, OH 44114, États-Unis",
                    "end_location": {
                        "lat": 41.4998051,
                        "lng": -81.69261209999999
                    },
                    "start_address": "50 Public Square, Cleveland, OH 44113, États-Unis",
                    "start_location": {
                        "lat": 41.4986974,
                        "lng": -81.6937592
                    },
                    "steps": [
                        {
                            "distance": {
                                "text": "23 pieds",
                                "value": 7
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 5
                            },
                            "end_location": {
                                "lat": 41.49869229999999,
                                "lng": -81.69384509999999
                            },
                            "html_instructions": "Prendre la direction <b>ouest</b> sur <b>S Roadway</b>",
                            "polyline": {
                                "points": "{eh|F~xrqN@B?D?F"
                            },
                            "start_location": {
                                "lat": 41.4986974,
                                "lng": -81.6937592
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "72 pieds",
                                "value": 22
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 25
                            },
                            "end_location": {
                                "lat": 41.4988885,
                                "lng": -81.6937969
                            },
                            "html_instructions": "Tourner à <b>droite</b> vers <b>E Roadway</b>",
                            "maneuver": "turn-right",
                            "polyline": {
                                "points": "yeh|FpyrqNYGMA"
                            },
                            "start_location": {
                                "lat": 41.49869229999999,
                                "lng": -81.69384509999999
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "259 pieds",
                                "value": 79
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 54
                            },
                            "end_location": {
                                "lat": 41.4995628,
                                "lng": -81.6938514
                            },
                            "html_instructions": "Tourner à <b>gauche</b> vers <b>E Roadway</b>",
                            "maneuver": "turn-left",
                            "polyline": {
                                "points": "agh|FfyrqNCFEDGBK@KAy@GKAEAE?C?A@A?A@"
                            },
                            "start_location": {
                                "lat": 41.4988885,
                                "lng": -81.6937969
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "328 pieds",
                                "value": 100
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 70
                            },
                            "end_location": {
                                "lat": 41.50007040000001,
                                "lng": -81.6928607
                            },
                            "html_instructions": "Tourner à <b>droite</b> vers <b>E Roadway</b>",
                            "maneuver": "turn-right",
                            "polyline": {
                                "points": "gkh|FpyrqNc@eAw@kBIS"
                            },
                            "start_location": {
                                "lat": 41.4995628,
                                "lng": -81.6938514
                            },
                            "travel_mode": "WALKING"
                        },
                        {
                            "distance": {
                                "text": "118 pieds",
                                "value": 36
                            },
                            "duration": {
                                "text": "1 minute",
                                "value": 39
                            },
                            "end_location": {
                                "lat": 41.4998051,
                                "lng": -81.69261209999999
                            },
                            "html_instructions": "Prendre <b>à droite</b> sur <b>E Roadway</b><div style=\"font-size:0.9em\">Votre destination se trouvera sur la gauche.</div>",
                            "maneuver": "turn-right",
                            "polyline": {
                                "points": "mnh|FjsrqN@A^]PQ"
                            },
                            "start_location": {
                                "lat": 41.50007040000001,
                                "lng": -81.6928607
                            },
                            "travel_mode": "WALKING"
                        }
                    ],
                    "traffic_speed_entry": [],
                    "via_waypoint": []
                }
            ],
            "overview_polyline": {
                "points": "{eh|F~xrqN@H?FYGMAILSDeAI[CC@e@cAaA_C`@_@PQ"
            },
            "summary": "S Roadway et E Roadway",
            "warnings": [
                "Le calcul d'itinéraires piétons est en bêta. Faites attention – Cet itinéraire n'est peut-être pas complètement aménagé pour les piétons."
            ],
            "waypoint_order": []
        }
    ],
    "status": "OK"
}
```

## Time Zone API

We'll try one more. The Time Zone API presents time zone information for a given set of coordinates. Again, the key doesn't seem to be required for testing, but you can [get a key](https://developers.google.com/maps/documentation/timezone/start#get-a-key) anyway if you'd like. FWIW, when I tried to re-use a *previous* auth key, it didn't like that - I got an error:

```json
{
    "errorMessage": "This API project is not authorized to use this API. Please ensure this API is activated in the Google Developers Console: https://console.developers.google.com/apis/api/timezone_backend?project=_",
    "status": "REQUEST_DENIED"
}
```

So for now, just omit the API key. To calculate the required timestamp parameter, which represents the number of seconds since the epoch, use the [Epoch Converter](https://www.epochconverter.com/)... assuming you can't just figure it out in your head. ;p Just copy the value where it says "The current Unix epoch time is ..."

    GET https://maps.googleapis.com/maps/api/timezone/json?location=41.4984174,-81.69372869999999&timestamp=1513992742

The results include the Time Zone ID and Name for the location you specified. Since I used a location in Cleveland OH, and daylight savings time ended in November, I get EST back.

```json
{
    "dstOffset": 0,
    "rawOffset": -18000,
    "status": "OK",
    "timeZoneId": "America/New_York",
    "timeZoneName": "Eastern Standard Time"
}
```

## Thoughts

I'm not sure why these calls didn't require an API key, since I'm not sure how they can enforce limits.

Speaking of limits, the limits for a free API key are very generous, typically thousands of requests per day for each API - certainly enough for personal use or even something small for a team at work - so that's nice.

Even though there are a lot of APIs, they seem to be very focused in purpose. It's interesting though, since the other APIs I've looked at are usually few (or one) with lots of endpoints that do different things. By breaking things up, it's probably easier to control access to one thing or another.

I'll definitely be looking for opportunites to try these out on some other projects - maybe when I start messing with the Raspberry Pi again. Can't imagine exactly what that'll be, but then that's the fun. :)
