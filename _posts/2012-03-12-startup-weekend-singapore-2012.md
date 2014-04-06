---
layout: post-no-feature
title: "Startup Weekend 2012"
description: "A retrospective of Startup Weekend 2012 Singapore"
category: articles
tags: [swsg, startup, weekend, retrospective]
---

![Startup Weekend 2012](/images/swsg/swsg.png)

I participated in Startup Weekend 2012 held at NUS Business School over the
weekend (9th to 11th March) and it was an absolute blast. It was great to
see so many people interested in pursuing their ideas and doing their
best to crank out a product by Sunday evening.

*Note*: This post is long and written in a single draft without much regard
for grammar and spelling accuracy.

#### Day 1: 9th March

Getting lost in NUS was expected so I left NTU slighty ahead of my original
schedule. Since there were no signs on the bus stops indicating where each
bus would head to, I had to walk around until I caught one of the bus
displaying a list of places it was going (Something -> SCI -> BIZ).

It was a couple minutes past six by the time I got to the venue. Dinner was
already served (lucky me) and some of the participants were already chatting
up with each other. There were different types of participants and we could
be identified by the colour of our lanyards.

+ Orange: Technical, Developers
+ Green: Designers
+ Dark blue: Business Development
+ Black: Sponsors or mentors (?)
+ Red: Organizers (NUS Overseas College Alumni members)

This gave those interested in getting developers to work on their ideas a
much easier way to identify the right person to pitch to. It was however
clear that some of the Developers or Designers were actually Business people
in disguise. Tickets for each role were limited and to get to the event they
had to buy technical / designer tickets.

Ideas to be worked on were posted on
[Pigeonhole](http://phlive.at/) and each of us would get five votes to choose
the ones we liked. The top fourty ideas would be selected and the suggester
would be given a chance to give a one minute pitch in front of everyone to
convince the judges why their should be worked on over the weekend. Out of the
fourty ideas, only twenty ideas would be chosen. Those twenty would each form
a team and the remaining participants can choose which one they would want to
work with.


![Startup Weekend 2012](/images/swsg/voting.jpg)
<small>Voting for top 40 ideas. Photo credt: Christina</small>

I was initially undecided during the pitches but I went ahead to try getting
into the 'TieTheKnot' team. Their idea was to develop and provide a platform
for couples to easily search for reliable wedding services, something which
I can somewhat relate to.

A quick chat with their main developer revealed that they were going to use
PHP and it was clear that I would be a misfit in that team. It has been years
since I last programmed in PHP and it would be nonsense if I say that I can
quickly pickup PHP again and start coding. This slight misfortune turned out to
be a blessing in disguise.

While cruising through the different groups, I happened to overhear
[Siu Rui](https://twitter.com/#!/siurui) telling one of the developers interested
in joining his team that they were going to use Python / Django. I ended up joining
them to work on the backend using Django.

The idea was to simplify the process of selling second hand items such as
electronics and textbooks. Traditionally you would have to take photos, upload
them online and post them on forums or classified listings sites like craiglists.
Now you simply have to "Snap" (take photos of the item), write some basic info
and "Sell". Buyers can offer you a price and you can opt to message or call the
buyer to further discuss the deal. For the purpose of the demo, we would demo
it as an iOS app.

At the end of the first day we discussed on the general administrative stuff
like exchanging numbers and emails and agreed to meet up the next day at 9am
to start work proper (it was 11+pm by then).

#### Day 2, 10th March

We first drafted out the main features of the application on Mahjong paper. Once
done the development team went on to discuss to the backend models and the
necessary API calls needed.

![Discussion](/images/swsg/discussion.jpg)
<small>Developer discussion. Photo credt: Christina</small>

The development team comprised of Alan, Lucas, Boon Kiat and myself. Alan and Lucas
focused on the front-end aspect by using PhoneGap to develop the main iOS
application. They coded and beautified the UI wonderfully and paid attention to
even minute details (someone even suggested doing a Path style app :p). The business
development team consisted of Christina, Mark, Shilpa and Siu Rui. Ideas, proposals
and suggestions were constantly brought up, discussed and shot down. Mark is the most
senior among us and his experience with both business and technical aspects enabled
us to rethink certain approaches, not to mention that he was also an occasional
logo / splash designer for us using Photoshop. Siu Rui helped design some of the
pages with Alan while Lucas focused on the PhoneGap API.

As previously mentioned, the backend was to be coded in Django. For demo purposes,
the models were simple enough:

+ Users: Default Django auth users with phone numbers as usernames.
+ Listing: Each user can post multiple listings and each would contain pictures along with
basic details.
+ Offer: Made by an user if they are interested in buying an item.

In the first iteration, an additional Picture class was available to allow multiple
listings in each Listing.

    Picture: Contain a picture for a Listing (Many pictures to one listing).

It was removed when the UI side decided to limit it to three pictures for the demo by
adding three ImageFields to the Listing class instead. This would simplify the JSON
serialization portion so that I do not have to serialize deep down into each foreign key.

The next step was to get the URLs up and going fast so that the UI side can have an API
ready when they need to test their Javascript code. Initially the JSON serializer built
into Django was used, but in order to return certain FK data I used the JSON serializer
on [wadofstuff](http://wadofstuff.googlecode.com/).

We designed a set of URLs to match the needs of the demo, such as `get-all-listings`,
`get-my-listings`, `get-offers-for-listing` etc. These functions were completed towards the
end of the day and received brutal testing by Alan and Lucas. We met a small hiccup
as the data returned was not JSONP but a simple hack which worked was found online by Alan.
We would get the callback parameter and wrap it around the JSON response.

Around midnight, we discovered that image loading on the listings screen was
simply way too slow. Turns out that each image uploaded and served was actually 1 MB each.
Alan and Lucas sought for a way to reduce the image resolution / size taken on by phone
but perhaps due to the toolkit issue (I am not so sure, seeking confirmation) it was decided
that that images will be resized on the server. I pulled out my code used for resizing in
another project and it worked pretty much okay after some fumbling with getting thumbnail
URLs to be serialized (django-stdimage library).

I left around 3.30am, Sunday while Alan, Lucas and Siu Rui continued to work on the project.
By some dumb blind luck, I woke up at the end of a REM cycle and felt strangely refreshed.

#### Last day, 11th March. 9am onwards

My dad drove me to back to the venue around 9am and since then I was pretty much on standby.
Alan worked til 6am to clear up some of the issues! Power level > 9000, man. Lucas and Siu
Rui took a short nap before and was already back in action.

![SnapSell FB Page](/images/swsg/snapsell.png)
<small>SnapSell Facebook Page.</small>

On Sunday I was mostly on Standby to fix the backend codes if necessarily and finding
memes faces for our slides, more on that later. We monitored whether API calls
were successful by observing the Django console. Due to large image file size, it was
possible to post a listing before the image was uploaded and this would cause 404s when
displaying a listing. We did not come up with a solution in time for that so we
had to actively monitor for Django output to check on the upload status.

It was also decided that we will display a Push Notification on the phone to simulate
a fake buyer interested in buying the item we have posted. It was clearly impractical to
use the iOS API at that point so I proposed a slightly simpler idea: the app
would poll the server constantly and it would display a hard coded notification upon
detecting a change. This naive implementation reduced the need for us to test additional
stuff and instead focus on getting the rest of the demo features up.

Hours and rehearsals later, it was time for us to pitch and demo our work.

![Pitching time](/images/swsg/demo.jpg)
<small>The SWSG Crowd during our pitch. Photo credt: Christina</small>

We were lucky in many aspects - the image uploads and notification were fast and on time!
It was mainly about the synchronization between Siu Rui (pitch-er) and Lucas (slides, demo),
it was flawless. We also showed tweets from people who were interested in our
ideas and that certainly helped us with customer validation _a lot_.
Monetization was a huge topic during the Q and A and I believed Siu Rui
addressed their concerns.

![Customer validation](/images/swsg/tweet.png)
<small>One of the tweets mentioned.</small>

Ultimately, we won! Victory in a competition does not imply actual business success, but
it certainly meant a lot of us after putting in 54 hours into it. At this point it is
still too early to say where things will go, but it does not hurt to give ourselves
a pat on the back for our efforts.

#### Misc rants / other technical stuff

Startup Weekend SG 2012 was my first SW style event so it was an eye opener. Event logistics
were great (food always arrive on time etc) and RedBull was a lifesaver.

However, the wireless connection provided was borderline un-usable when all participants were in.
I was unable to access the net and SSH my servers; thankfully Boon Kiat tethered his phone to
give me a connection to deploy. I probably wrote the one of the worst Django
codes possible during this event but.. *it works*.  It is during this kind of
event that you need to seek not perfection but worka-bility.

Our application architecture was as follows:

+ Frontend: HTML + Jquery Mobile site deployed as an iOS app using PhoneGap.
+ Backend: Basic Django. Uses `django-stdimage` for resizing images and the JSON serializer in wadofstuff.

The backend server was deployed on my VPS (located in SG). I attempted to deploy a backup to EC2 but
there was not enough time to guarantee its usability. The frontend app was directly deployed onto
an iPhone.

+ Slides: Keynote -> PDF -> iBooks

For seamless transition between the slides and product demo, we used iBooks to render the slides
through the VGA adapter. The multi-LCD screens take way too long to reinitialize itself when switching
laptops so future iOS product demos can consider using this presentation method.

### And.. the team.

![SnapSell Team](/images/swsg/team.jpg)
<small>The SnapSell Team. Photo credit: Christina</small>

From left to right, background: Myself, Boon Kiat, Lucas, Siu Rui, Alan, Mark
From left to right, foreground: Christina, Shilpa.

It was awesome to win, but it was the team spirit that made SWSG really really *awesome*.
