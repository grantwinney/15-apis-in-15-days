<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---Trello-API.jpg" width=500>

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/), today I'm writing about the [Trello API](https://developers.trello.com/v1.0/reference). If you're unfamiliar with Trello, it's a nice service that's similar to a kanban board... or basically a to-do list if you're just using it personally like I do. Anyway, I like it - maybe you will too.

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

---

## Authorization

The process for trying things out is nice and easy. Go to the [Developer API Keys](https://trello.com/app-key) screen and copy the "Key" at the top of the page. Then click where it says "generate a Token" (it requests access to everything so you can try everything) and copy the token it generates too.

<img src="https://grantwinney.com/content/images/2017/12/trello-api---create-token.png" width="400">

## Trying It Out

Every request needs to have the key and auth token you copied above. To try it out, create a new board, and then throw a few lists in there, and a few cards in each list. You might want to add a couple descriptions and attachments too. Doing that will make it easier to see how the data is returned to you.

### Get Board Metadata

Open your sample board and check out the URL. Copy the unique 8-character ID and [retrieve your board's metadata](https://developers.trello.com/v1.0/reference#boardsboardid-1).

    GET https://api.trello.com/1/boards/Ii6vhIyq?fields=name,url&key=<your-key>&token=<your-token>

The ID is returned, but apparently isn't necessary since the 8-character code seems to work just fine for identifying a board too (after all, that still allows for 218 *trillion* combos).

```json
{
    "id": "5a43c77676a7de01ddf92550",
    "name": "Just a test board",
    "url": "https://trello.com/b/Ii6vhIyq/just-a-test-board"
}
```

### Get Cards for a Board

What if you want to [get the contents of a board](https://developers.trello.com/v1.0/reference#boardsboardidtest), not just metadata? You can request that too.

    https://api.trello.com/1/boards/<board-id>/cards?key=<your-key>&token=<your-token>

Here's part of the result from my sample board. I've actually got 5 cards, but I'm only showing data for 2 of them. There's a lot of unique IDs we can use to look up more information. The first card below has an image attached - see where it has a value for `idAttachmentCover` where the second card has null? The second card has a description (see `desc`), checklist (see `idChecklists`), and an attachment (a link) which doesn't seem to show in the response. Am I missing something?

```json
[
    {
        "id": "5a43c8080110176567dbd1d5",
        "checkItemStates": null,
        "closed": false,
        "dateLastActivity": "2017-12-27T16:20:39.902Z",
        "desc": "",
        "descData": null,
        "idBoard": "5a43c77676a7de01ddf92550",
        "idList": "5a43c7fb728185304d89c4ac",
        "idMembersVoted": [],
        "idShort": 2,
        "idAttachmentCover": "5a43c85796edd0379fa357ce",
        "idLabels": [],
        "manualCoverAttachment": false,
        "name": "first list, card 2",
        "pos": 131071,
        "shortLink": "QSWX0iug",
        "badges": {
            "votes": 0,
            "attachmentsByType": {
                "trello": {
                    "board": 0,
                    "card": 0
                }
            },
            "viewingMemberVoted": false,
            "subscribed": false,
            "fogbugz": "",
            "checkItems": 0,
            "checkItemsChecked": 0,
            "comments": 0,
            "attachments": 1,
            "description": false,
            "due": null,
            "dueComplete": false
        },
        "dueComplete": false,
        "due": null,
        "idChecklists": [],
        "idMembers": [],
        "labels": [],
        "shortUrl": "https://trello.com/c/QSWX0iug",
        "subscribed": false,
        "url": "https://trello.com/c/QSWX0iug/2-first-list-card-2"
    },
    {
        "id": "5a43c80fa2cc50186c1f8df9",
        "checkItemStates": null,
        "closed": false,
        "dateLastActivity": "2017-12-27T17:10:06.844Z",
        "desc": "some brief description",
        "descData": {
            "emoji": {}
        },
        "idBoard": "5a43c77676a7de01ddf92550",
        "idList": "5a43c7feffff9c77011ab8c0",
        "idMembersVoted": [],
        "idShort": 4,
        "idAttachmentCover": null,
        "idLabels": [],
        "manualCoverAttachment": false,
        "name": "second list, card 2",
        "pos": 131071,
        "shortLink": "Ko9IqGHj",
        "badges": {
            "votes": 0,
            "attachmentsByType": {
                "trello": {
                    "board": 0,
                    "card": 0
                }
            },
            "viewingMemberVoted": false,
            "subscribed": false,
            "fogbugz": "",
            "checkItems": 3,
            "checkItemsChecked": 1,
            "comments": 0,
            "attachments": 1,
            "description": true,
            "due": null,
            "dueComplete": false
        },
        "dueComplete": false,
        "due": null,
        "idChecklists": [
            "5a43d3805d56ec37f5780b4e"
        ],
        "idMembers": [],
        "labels": [],
        "shortUrl": "https://trello.com/c/Ko9IqGHj",
        "subscribed": false,
        "url": "https://trello.com/c/Ko9IqGHj/4-second-list-card-2"
    },
]
```

### Get Details for a Card

In Trello, boards have lists, lists have cards, and cards have ... stuff. Like [checklists](https://developers.trello.com/v1.0/reference#checklistsid), [attachments](https://developers.trello.com/v1.0/reference#cardsidattachments), etc.  Here's what the card I referenced above, with the checklists and whatnot, looks like:

<img src="https://grantwinney.com/content/images/2017/12/trello-api---card-1.png" width="500"><br>

There are endpoints to query all of that data. Let's query the checklist.

    GET https://api.trello.com/1/checklists/5a43d3805d56ec37f5780b4e?key=<your-key>&token=<your-token>

```json
{
    "id": "5a43d3805d56ec37f5780b4e",
    "name": "Vegetables",
    "idBoard": "5a43c77676a7de01ddf92550",
    "idCard": "5a43c80fa2cc50186c1f8df9",
    "pos": 16384,
    "checkItems": [
        {
            "state": "complete",
            "idChecklist": "5a43d3805d56ec37f5780b4e",
            "id": "5a43d387c33b6b72c540fcaa",
            "name": "Carrots",
            "nameData": {
                "emoji": {}
            },
            "pos": 17286
        },
        {
            "state": "incomplete",
            "idChecklist": "5a43d3805d56ec37f5780b4e",
            "id": "5a43d3895da5b8803a9ebf80",
            "name": "Beans",
            "nameData": {
                "emoji": {}
            },
            "pos": 34431
        },
        {
            "state": "incomplete",
            "idChecklist": "5a43d3805d56ec37f5780b4e",
            "id": "5a43d3e261265ba4b7c9ff0d",
            "name": "Tomatoes?",
            "nameData": null,
            "pos": 51275
        }
    ]
}
```

### Update a Card

We can also [update cards](https://developers.trello.com/v1.0/reference#cardsid-1). Here's a `PUT` request that updates the above card's name and description:

    PUT https://api.trello.com/1/cards/5a43c80fa2cc50186c1f8df9?name=second%20list,%20card%20TOOOO&desc=a%20less%20briefer%20description&key=<your-key>&token=<your-token>

The response returns the entire updated record, which is pretty typical for a REST endpoint (but not required... in fact, the suggestions and recommendations for REST APIs are endless, but there are very few hard and fast rules). Here's what the card looks like after the update:

<img src="https://grantwinney.com/content/images/2017/12/trello-api---card-2.png" width="500">

## Thoughts

The [documentation](https://developers.trello.com/v1.0/reference) is pretty easy to read. There are tons of endpoints for getting as much or as little data as you need, and you can even try most of them out right from their website.

I like that they make it so easy to get an auth token with access to everything, for the purposes of playing around, but that they provide fine-grained access for when you get around to creating a real application. When you're designing an app, it should only request access to what it absolutely needs.

There's way more available than I could possibly even touch the tip of the iceberg with here. There are enough calls to allow you to write some pretty cool apps / browser extensions / whatever. The devs behind it did a really nice job. Now that [Atlassian purchased Trello](https://techcrunch.com/2017/01/09/atlassian-acquires-trello/), I'm hoping it continues to improve where needed - and remain the same where it already works perfectly well!
