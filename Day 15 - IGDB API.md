<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---igdb-api.jpg" width=500>

Wrapping up my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/), the last API I'll write about is one I found completely at random - the [IGDB API](https://www.igdb.com/api). The Internet Game Database (IGDB) is a community-driven site that collects and shares information about games and game-related data.

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

## Authentication

You have to [sign up for an account](https://api.igdb.com/signup) in order to get a key. Open the email they send, click on the link in it and login again, and note two pieces of information you'll need in order to make requests - the key (on the right), and the request url (on the left) that takes the form of `https://api-<random-number>.apicast.io`.

![igdb---api-key](https://grantwinney.com/content/images/2017/12/igdb---api-key.png)

## Try It Out

Every request should have two headers:

* `Accept: application/json`
* `user-key: <your-api-key>`

And the base "request" url is:

* `https://api-<random-number>.apicast.io`

## Find Games

You can request games by keyword, limiting the results to just the ID and Name, so you can lookup more details later.

    GET https://<your-request-url>/games/?search=zelda&fields=id,name

Here's what I got back with the above search for Zelda games:

```json
[
    {
        "id": 1025,
        "name": "Zelda II: The Adventure of Link"
    },
    {
        "id": 42308,
        "name": "The Legend of Zelda: Ancient Stone Tablets"
    },
    {
        "id": 45143,
        "name": "The Legend of Zelda: Four Swords Anniversary Edition"
    },
    {
        "id": 1034,
        "name": "The Legend of Zelda: Four Swords Adventures"
    },
    {
        "id": 45141,
        "name": "The Legend of Zelda: Majora's Mask Collector's Edition"
    },
    {
        "id": 1027,
        "name": "The Legend of Zelda: Link's Awakening DX"
    },
    {
        "id": 2909,
        "name": "The Legend of Zelda: A Link Between Worlds"
    },
    {
        "id": 1026,
        "name": "The Legend of Zelda: A Link to the Past"
    },
    {
        "id": 41829,
        "name": "The Legend of Zelda: Breath of the Wild - Expansion Pass"
    },
    {
        "id": 45138,
        "name": "duplicate The Legend of Zelda: Breath of the Wild Limited Edition"
    }
]
```

## Find Game Details

And if you want to look up the details of a specific game, like "The Legend of Zelda: A Link to the Past", then just plug its ID into a request like this. You can use omit `fields` to get everything, but if you're only interested in a few pieces of data then you can also specify which fields you want.

    GET https://<your-request-url>/games/1026?fields=id,name,url,summary,storyline,websites

```json
[
    {
        "id": 1026,
        "name": "The Legend of Zelda: A Link to the Past",
        "url": "https://www.igdb.com/games/the-legend-of-zelda-a-link-to-the-past",
        "summary": "Link, a blacksmith's nephew living in Hyrule, must free the land from the evildoings of Ganon. Link must take up the mythical Master Sword and collect the three Triforces in order to free the Seven Maidens, including the princess of Hyrule, Zelda, from the dungeons and castles of the Dark World to stop Ganon.",
        "storyline": "The plot of A Link to the Past focuses on Link as he travels on a journey to save Hyrule, defeat Ganon and rescue the seven descendants of the Sages. A Link to the Past uses a 3/4 top-down perspective similar to that of the original The Legend of Zelda, dropping the side scrolling elements of Zelda II: The Adventure of Link. A Link to the Past introduced elements to the series that are still commonplace today, such as the concept of an alternate or parallel world, the Master Sword and other new weapons and items.",
        "websites": [
            {
                "category": 3,
                "url": "https://en.wikipedia.org/wiki/The_Legend_of_Zelda:_A_Link_to_the_Past"
            },
            {
                "category": 1,
                "url": "http://zelda.com/universe/game/past/"
            }
        ]
    }
]
```

## Thoughts

It's a nice API. You can read more about [all the available endpoints](https://igdb.github.io/api/endpoints/) - there seem to be quite a few others for looking up information about game developers, development companies, reviews, etc.

It's been an interesting couple of weeks, looking up a ton of different APIs, but I'm *done*. It's been exhausting trying to research so many APIs, but I learned quite a bit. I had no idea there was so much (free) data available, from hobby sites, companies, and governmental agencies alike. Nearly any data you're interested in querying and manipulating is available somewhere out there...

If you find anything interesting yourself, let me know about it! I'd be interested in checking it out, or hearing about how you made good use of it. Good luck!
