<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---HubbleSite-API.jpg" width="500">

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/), today I'm checking out another space-themed API - the [HubbleSite API](http://hubblesite.org/api/documentation/).

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

---

## What is HubbleSite?

When I stumbled across this API, I thought it might just be an amateur site, but  it's more interesting than that. Here's a little about the [Space Telescope Science Institute](http://www.stsci.edu/institute/), maintainers of HubbleSite and this API (excerpted from their site):

> Since its launch in 1990, we have performed the science operations for Hubble. We also contribute to other NASA missions and work with various international partners. Our staff conducts world-class scientific research, engineering, and mission support; our Barbara A. Mikulski Archive for Space Telescopes holds and disseminates data from over 20 astronomical missions; and we bring science to the world through internationally recognized news, education, and public outreach programs.

## Authorization

Obtaining an API (or authorization) token is a normal prereq for using most APIs, but there isn't one for the HubbleSite API. They perform some level of caching though, so I assume a brand new image or news article might not be available immediately.

## Using the API

You can access all their [news](http://hubblesite.org/news), [images](http://hubblesite.org/images/gallery), and [videos](http://hubblesite.org/videos/science) right through their website, but using the API to get a JSON response allows you to manipulate it as you'd like, perhaps to create a random "photo of the day" on your own site. *(crediting the original source of course!)*

### Images

You can access images with a simple `GET` request to `http://hubblesite.org/api/v3/images`, but the API allows you to filter your results to a *type* of photo too - for example, "holiday_cards", "wallpaper", "spacecraft", "news", "printshop", "stsci_gallery", etc. I have absolutely no idea what they all mean and I don't see them defined, but there ya go...

Here's a request to get all spacecraft photos, for example:

    GET http://hubblesite.org/api/v3/images?page=all&collection_name=spacecraft

The results include a handful of photos from the May 2009 repair/upgrade mission on the Hubble, which Astronaut Mike Massimino talked about in his book, [Spaceman](https://grantwinney.com/book-review-spaceman-by-mike-massimino/) - if you haven't read that, by the way, I *highly* recommend it. He's very personable, and shows the human side of NASA.

```json
[
    {
        "id": 3814,
        "name": "Grappling Hubble (2009)"
    },
    {
        "id": 3813,
        "name": "Bidding Hubble Farewell (2009)"
    },
    {
        "id": 3812,
        "name": "Hubble From Behind (2009)"
    },
    {
        "id": 3811,
        "name": "Installing Wide Field Camera 3 (2009)"
    },
    {
        "id": 3810,
        "name": "Final Release Over Earth (2009)"
    },
    ...
    ...
]
```

With the "id", you can get the details of a particular photo, along with several URLs for different sizes/formats, and then use that data for your own nefarious purposes. ;)

    GET http://hubblesite.org/api/v3/image/3811

```json
{
    "name": "Installing Wide Field Camera 3 (2009)",
    "description": "Astronaut Andrew Feustel, perched on the end of the robotic arm, helps to install the Wide Field Camera 3 (WFC3) during a May 14, 2009, spacewalk to perform work on the Hubble Space Telescope. Wide Field Camera 3 is one of two new instruments installed during Servicing Mission 4, the fifth astronaut visit to Hubble.",
    "credits": "<a href=\"http://www.nasa.gov\">NASA</a>",
    "mission": "hubble",
    "collection": "spacecraft",
    "image_files": [
        {
            "file_url": "https://media.stsci.edu/uploads/image_file/image_attachment/29279/STScI-H-spacecraft27-title.pdf",
            "file_size": 4551893,
            "width": 792,
            "height": 612
        },
        {
            "file_url": "https://media.stsci.edu/uploads/image_file/image_attachment/29278/STScI-H-spacecraft27-title-3000x2400.jpg",
            "file_size": 1235602,
            "width": 3000,
            "height": 2400
        },
        {
            "file_url": "https://media.stsci.edu/uploads/image_file/image_attachment/29276/STScI-H-spacecraft27-3070x2036.jpg",
            "file_size": 3963387,
            "width": 3070,
            "height": 2036
        },
        {
            "file_url": "https://media.stsci.edu/uploads/image_file/image_attachment/29277/STScI-H-spacecraft27-3070x2036.tif",
            "file_size": 18797484,
            "width": 3070,
            "height": 2036
        }
    ]
}
```

<img src="https://grantwinney.com/content/images/2017/12/STScI-H-spacecraft27-title-3000x2400.jpg" width="500"><br>

<img src="https://grantwinney.com/content/images/2017/12/STScI-H-spacecraft29-title-2400x3000.jpg" width="500"><br>

<img src="https://grantwinney.com/content/images/2017/12/STScI-H-spacecraft30-title-3000x2400.jpg" width="500"><br>

<img src="https://grantwinney.com/content/images/2017/12/STScI-H-spacecraft32-title-3000x2400.jpg" width="500">

## Thoughts

Their [documentation](http://hubblesite.org/api/documentation/) is an easy read. It shows you how to access their videos and news releases too, which is pretty much the same way as images. There's some space-related terms they built up into a dictionary too, but it's pretty small and seems of limited use, unless you used it to create a "term of the day" on your site or something.

Not much else to say. Today's was a small API (compared to some others), but it's awesome they put in the effort to make their data - photos, videos, etc - freely and easily accessible to anyone who can find a use for them!
