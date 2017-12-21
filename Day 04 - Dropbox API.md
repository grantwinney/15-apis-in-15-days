<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---day-04-dropbox.jpg" width=500>

Continuing my [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/) challenge, today I played around with the [Dropbox API](https://www.dropbox.com/developers/), or DBX as they apparently call it. Dropbox provides file storage that syncs between your devices, but also appears to provide for synchronized collaboration of individual files on teams. Neato.

First though, two things to consider:

* If you're unfamiliar with APIs, [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself with the concept.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

---

## API Explorer

This is pretty cool, and something I didn't see with the other APIs I've looked at. Dropbox developed an [API Explorer](https://dropbox.github.io/dropbox-api-v2-explorer/) that lets you try various endpoints right through the website. Just choose an endpoint, click the "Get Token" button *(you need to have an account and be signed in),* and then fill in whatever details are required for the endpoint you selected. It even links to the relevant documentation, and code you can use outside of the API Explorer... very handy.

### List Contents of a Folder

Here's the [list_folder](https://dropbox.github.io/dropbox-api-v2-explorer/#files_list_folder) API call (leave all the fields empty except access token), which shows the only two files in my account - the ones Dropbox added when I signed up.

```json
{
  "entries": [
    {
      ".tag": "file",
      "name": "Get Started with Dropbox.pdf",
      "path_lower": "/get started with dropbox.pdf",
      "path_display": "/Get Started with Dropbox.pdf",
      "id": "id:XhBonzeH_8AAAAAAAAAABQ",
      "client_modified": "2017-12-18T00:15:27Z",
      "server_modified": "2017-12-18T00:15:28Z",
      "rev": "17769f930",
      "size": 1102331,
      "content_hash": "f7ad488deb7d81790340ecd676fe6e47f0a6064fb99b982685b752d58611c1cb"
    },
    {
      ".tag": "file",
      "name": "Get Started with Dropbox Paper.url",
      "path_lower": "/get started with dropbox paper.url",
      "path_display": "/Get Started with Dropbox Paper.url",
      "id": "id:XhBonzeH_8AAAAAAAAAABg",
      "client_modified": "2017-12-18T00:15:28Z",
      "server_modified": "2017-12-18T00:15:28Z",
      "rev": "27769f930",
      "size": 81,
      "content_hash": "16f386add4634a2e6e5a7fc782c51131a5347b9aabcc3cade0bc6c8bf7e304d9"
    }
  ],
  "cursor": "AAFFJ-wCdYaJb11Vghos85aOI3eVAmvpJolSrVtaPev6PlXlRPPjpkAjdwl7eZoe33qTGoL2XuKDxzd-fNXuqMGBiy4JLu8nrsiDP9zRHCBXoQXfQWmgLUBo9U9vBp003hff6bMSBHpsSQ5dGovH5kSd",
  "has_more": false
}
```

### Create a New Folder

You can [create a new folder](https://dropbox.github.io/dropbox-api-v2-explorer/#files_create_folder):

![dropbox-api---create-folder](https://grantwinney.com/content/images/2017/12/dropbox-api---create-folder.png)

![dropbox-api---folder-created](https://grantwinney.com/content/images/2017/12/dropbox-api---folder-created.png)

### Move a File

Now [move a file](https://dropbox.github.io/dropbox-api-v2-explorer/#files_move) into the folder you just created:

![dropbox-api---move-file](https://grantwinney.com/content/images/2017/12/dropbox-api---move-file.png)

![dropbox-api---file-moved](https://grantwinney.com/content/images/2017/12/dropbox-api---file-moved.png)

## Authenticating

In order to play around _without_ using their API Explorer, you'll need an access token. You can either copy the one that the API Explorer generated for you, or you can create a new app.

If you decide to create a new app, [go here](https://www.dropbox.com/developers/apps), press "Create app", choose "Dropbox API" and "Full Dropbox", then smash a bunch of keys for the name of your app - because apparently it has to be a unique name among every app anyone has ever made. 😕

Press "Create app" and on the next page you'll find a section that lets you generate an access token.

![dropbox-api---create-app](https://grantwinney.com/content/images/2017/12/dropbox-api---create-app.png)

## Trying it out

The API Explorer also includes the actual code you can use to make the call on your own. Look for the "Show Code" button and click it.

*You'll need these headers specified for the following examples:*

![dropbox-api---headers](https://grantwinney.com/content/images/2017/12/dropbox-api---headers.png)

### List Contents of a Folder

Check out the [list folder](https://dropbox.github.io/dropbox-api-v2-explorer/#files_list_folder) endpoint again in the API Explorer. Click the Show Code button and you should see something similar to this. You might even want to enter some values and watch how the code block is updated.

```none
curl -X POST https://api.dropboxapi.com/2/files/list_folder \
  --header 'Authorization: Bearer null' \
  --header 'Content-Type: application/json' \
  --data '{"path":""}
```

You can use that to construct a call in Postman. You can also get the same info from the [docs](https://www.dropbox.com/developers/documentation/http/documentation#files-list_folder), but it's great that you can tweak the values in the API Explorer, and when you're happy with the results then you can copy the "Show Code" section.

I decided to list all folders recursively from the very top.

![dropbox-api---postman-list-folder](https://grantwinney.com/content/images/2017/12/dropbox-api---postman-list-folder.png)

### Move a File

Let's try moving a file again. I grabbed this code from the [API Explorer](https://dropbox.github.io/dropbox-api-v2-explorer/#files_move) too.

![dropbox-api---move-file-postman-2](https://grantwinney.com/content/images/2017/12/dropbox-api---move-file-postman-2.png)

## Thoughts

The API Explorer is a really cool tool. It's a good example of dogfooding, where developers use their own code (the API endpoints) to make a tool they can also use (the API Explorer). It also allows you to find the exact endpoint you're interested in, then find links to documentation and the code needed to implement it outside the explorer.

They have a lot of [examples in various languages](https://www.dropbox.com/developers/documentation). I briefly checked out the [.NET examples](https://www.dropbox.com/developers/documentation/dotnet#tutorial), and they even offer a Dropbox.NET SDK to make development far easier. It's more friendly looking than having to call a REST endpoint directly in code.

```csharp
static async Task Run()
{
    using (var dbx = new DropboxClient("YOUR ACCESS TOKEN"))
    {
        var full = await dbx.Users.GetCurrentAccountAsync();
        Console.WriteLine("{0} - {1}", full.Name.DisplayName, full.Email);
    }
}
```
