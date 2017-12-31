<img src="https://grantwinney.com/content/images/2017/12/15-apis-in-15-days---penguin-random-house.jpg" width="500">

Continuing my search for [15 APIs in 15 Days](https://grantwinney.com/a-new-challenge-15-apis-in-15-days/), today I'm writing about the [Penguin Random House API](http://www.penguinrandomhouse.biz/webservices/rest/). Penguin is a book publisher, and their API can be used to get data about books, authors and events.

First though, two things to consider:

* If you're unfamiliar with APIs, you might want to [read this first](https://grantwinney.com/what-is-an-api/) to familiarize yourself.
* Install [Postman](https://www.getpostman.com/), which allows you to access API endpoints without having to write an app, as well as save the calls you make and sync them online.

Normally the first thing you have to worry about is some form of authorization and getting an API key. This API doesn't require or even offer it though, so... let's make some requests!

## Find Authors

Try looking up an author such as Isaac Asimov. You can specify a first and last name, and most likely you'll get multiple `author` records back - especially if you search for a popular name like "Smith" or something.

    GET https://reststop.randomhouse.com/resources/authors?lastName=Asimov&firstName=Isaac

Here's a very small sample of the returned payload. You get the name back as well as an `authorid` (we'll use that in a moment), a short bio, and a list of titles (Asimov wrote a *lot* of books).

```json
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<authors uri="https://reststop.randomhouse.com/resources/authors?lastName=Asimov&amp;firstName=Isaac">
    <author uri="https://reststop.randomhouse.com/resources/authors/947">
        <approved>X</approved>
        <authordisplay>Isaac Asimov</authordisplay>
        <authorfirst>Isaac</authorfirst>
        <authorfirstlc>isaac</authorfirstlc>
        <authorid>947</authorid>
        <authorlast>Asimov</authorlast>
        <authorlastfirst>ASIMOV, ISAAC</authorlastfirst>
        <authorlastlc>asimov</authorlastlc>
        <titles>
            <isbn contributortype="A">9780307488633</isbn>
            <isbn contributortype="A">9780307490247</isbn>
            <isbn contributortype="A">9780307573537</isbn>
            ...
        </titles>
        <lastinitial>a</lastinitial>
        <spotlight>&lt;b&gt;Isaac Asimov&lt;/b&gt;&amp;nbsp;began his Foundation series at the age of 21, not realizing that it would one day be considered a cornerstone of science fiction. During his legendary career, Asimov penned over 470 books on subjects ranging from science to Shakespeare to history, though he was most loved for his award-winning science fiction sagas, which include the Robot, Empire, and Foundation series. Named a Grand Master of Science Fiction by the Science Fiction and Fantasy Writers of America, Asimov entertained and educated readers of all ages for close to five decades. He died, at the age 72, in April 1992.</spotlight>
        <works>
            <works>5595</works>
            <works>5596</works>
            <works>5597</works>
            ...
        </works>
    </author>
    ...
</authors>
```

## Get Author Details

You can use the `authorid` value from the previous response (or just use the `uri` attribute of the `author` node) to get the details of the author you're interested in.

    GET https://reststop.randomhouse.com/resources/authors/947

What I find interesting is that it seems to be the same data as the previous request. I'm not sure why, in order to make it faster, the previous endpoint doesn't return less data... maybe a few titles and works to make sure it's the right author in the event of some ambiguity.

```json
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<author uri="https://reststop.randomhouse.com/resources/authors/947">
    <approved>X</approved>
    <authordisplay>Isaac Asimov</authordisplay>
    <authorfirst>Isaac</authorfirst>
    <authorfirstlc>isaac</authorfirstlc>
    <authorid>947</authorid>
    <authorlast>Asimov</authorlast>
    <authorlastfirst>ASIMOV, ISAAC</authorlastfirst>
    <authorlastlc>asimov</authorlastlc>
    <titles>
        <isbn contributortype="A">9780307488633</isbn>
        <isbn contributortype="A">9780307490247</isbn>
        <isbn contributortype="A">9780307573537</isbn>
        ...
    </titles>
    <lastinitial>a</lastinitial>
    <spotlight>&lt;b&gt;Isaac Asimov&lt;/b&gt;&amp;nbsp;began his Foundation series at the age of 21, not realizing that it would one day be considered a cornerstone of science fiction. During his legendary career, Asimov penned over 470 books on subjects ranging from science to Shakespeare to history, though he was most loved for his award-winning science fiction sagas, which include the Robot, Empire, and Foundation series. Named a Grand Master of Science Fiction by the Science Fiction and Fantasy Writers of America, Asimov entertained and educated readers of all ages for close to five decades. He died, at the age 72, in April 1992.</spotlight>
    <works>
        <works>5595</works>
        <works>5596</works>
        <works>5597</works>
        ...
    </works>
</author>
```

## Get Title / Work Details

The two responses so far have included the author's titles and works, of which I just showed a few (Asimov had hundreds). You can use that data with another endpoint to get more details about them. FWIW, I'm a little fuzzy on the difference between "titles" and "works"...

    GET https://reststop.randomhouse.com/resources/titles/9780307490247

```json
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<title uri="https://reststop.randomhouse.com/resources/titles/9780307490247">
    <author>ASIMOV, ISAAC</author>
    <authors>
        <authorId contributortype="A">947</authorId>
    </authors>
    <authorbio>&lt;b&gt;Isaac Asimov&lt;/b&gt; began his Foundation series at the age of twenty-one, not realizing that it would one day be considered a cornerstone of science fiction. During his legendary career, Asimov penned more than 470 books on subjects ranging from science to Shakespeare to history, though he was most loved for his award-winning science fiction sagas, which include the Robot, Empire, and Foundation series. Named a Grand Master of Science Fiction by the Science Fiction Writers of America, Asimov entertained and educated readers of all ages for close to five decades. He died, at the age of seventy-two, in April 1992.</authorbio>
    <authorweb>Isaac Asimov</authorweb>
    <awards/>
    <characters/>
    <contributorfirst1>Isaac</contributorfirst1>
    <contributorlast1>Asimov</contributorlast1>
    <division>Del Rey</division>
    <flapcopy>A millennium into the future two advances have altered the course of human history: the colonization of the Galaxy and the creation of the positronic brain. Isaac Asimov's Robot novels chronicle the unlikely partnership between a New York City detective and a humanoid robot who must learn to work together.&lt;br&gt;&lt;br&gt;Detective Elijah Baiey is called to the Spacer world Aurora to solve a bizarre case of roboticide. The prime suspect is a gifted roboticist who had the means, the motive, and the opportunity to commit the crime. There's only one catch: Baley and his positronic partner, R. Daneel Olivaw, must prove the man innocent. For in a case of political intrigue and love between woman and robot gone tragically wrong, there's more at stake than simple justice. This time Baley's career, his life, and Earth's right to pioneer the Galaxy lie in the delicate balance.</flapcopy>
    <formatcode>EL</formatcode>
    <formatname>eBook</formatname>
    <imprint>Spectra</imprint>
    <isbn>9780307490247</isbn>
    <isbn10>0307490246</isbn10>
    <isbn10hyphenated>0-307-49024-6</isbn10hyphenated>
    <isbn13hyphenated>978-0-307-49024-7</isbn13hyphenated>
    <keyword>The Robots of Dawn :  : Isaac Asimov : Spectra : Fiction - Science Fiction - Hard Science Fiction : Fiction - Classics : Fiction - Science Fiction - Space Opera : 0307490246 : 0-307-49024-6 : 9780307490247 : 978-0-307-49024-7 : A millennium into the future two advances have altered the course of human history: the colonization of the Galaxy and the creation of the positronic brain. Isaac Asimov's Robot novels chronicle the unlikely partnership between a New York City detective and a humanoid robot who must learn to work together.&lt;br&gt;&lt;br&gt;Detective Elijah Baiey is called to the Spacer world Aurora to solve a bizarre case of roboticide. The prime suspect is a gifted roboticist who had the means, the motive, and the opportunity to commit the crime. There's only one catch: Baley and his positronic partner, R. Daneel Olivaw, must prove the man innocent. For in a case of political intrigue and love between woman and robot gone tragically wrong, there's more at stake than simple justice. This time Baley's career, his life, and Earth's right to pioneer the Galaxy lie in the delicate balance.</keyword>
    <onsaledate>01/21/2009</onsaledate>
    <pages>448</pages>
    <pricecanada>8.99</pricecanada>
    <priceusa>7.99</priceusa>
    <relatedisbns>
        <isbn formatcode="MM">9780553299496</isbn>
        <isbn formatcode="EL">9780307490247</isbn>
        <isbn formatcode="DN">9780804191241</isbn>
    </relatedisbns>
    <salestatus>EL</salestatus>
    <subformat>001</subformat>
    <subjectcategory1>FIC028020</subjectcategory1>
    <subjectcategory2>FIC004000</subjectcategory2>
    <subjectcategory3>FIC028030</subjectcategory3>
    <subjectcategorydescription1>Fiction - Science Fiction - Hard Science Fiction</subjectcategorydescription1>
    <subjectcategorydescription2>Fiction - Classics</subjectcategorydescription2>
    <subjectcategorydescription3>Fiction - Science Fiction - Space Opera</subjectcategorydescription3>
    <tgpdf>false</tgpdf>
    <themes/>
    <titleauthisbn>The Robots of Dawn : Isaac Asimov : 0307490246 : 0-307-49024-6 : 9780307490247 : 978-0-307-49024-7</titleauthisbn>
    <titleshort>ROBOTS OF DAWN, THE (EBK)</titleshort>
    <titlesubtitleauthisbn>The Robots of Dawn :  : Isaac Asimov : 0307490246 : 0-307-49024-6 : 9780307490247 : 978-0-307-49024-7</titlesubtitleauthisbn>
    <titleweb>The Robots of Dawn</titleweb>
    <updatedOn>2017-12-02T01:59:13.000</updatedOn>
    <webdomains>
        ...
    </webdomains>
    <links/>
    <workid>5799</workid>
</title>
```

    GET https://reststop.randomhouse.com/resources/works/5596
    
```json
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<work uri="https://reststop.randomhouse.com/resources/works/5596">
    <authorweb>ASIMOV, ISAAC</authorweb>
    <titles>
        <isbn formatcode="TR">9780449900482</isbn>
    </titles>
    <onsaledate>1981-09-12T00:00:00-04:00</onsaledate>
    <titleAuth>A Choice of Catastrophes : Isaac Asimov</titleAuth>
    <titleSubtitleAuth>A Choice of Catastrophes : The Disasters That Threaten Our World : Isaac Asimov</titleSubtitleAuth>
    <titleshort>CHOICE OF CATASTROPHES, A</titleshort>
    <titleweb>A Choice of Catastrophes</titleweb>
    <workid>5596</workid>
</work>
```

## Thoughts

There's not too much to say about this one. Not sure what the limitations / request throttling might be for this API. It's free, which is nice! The documentation is all in one place and includes examples in php and java, which is nice too.

One more day to go...
