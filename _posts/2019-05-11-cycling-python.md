---
layout: post
title: Bike Racing - With A Little Help From Python
author: "Jordan Calcutt"
categories: journal
tags: [python, pandas, blog]
image: bike_race.png
---
<b>
Racing bikes is tough.
From my experience, to be successful in a race, it takes something
like: 70% fitness, 20% tactics and 10% luck.
</b>

I’ve been racing bikes for a few years now.
I enjoy the adrenaline of riding fast, the social aspect of it, and the
feeling when hours of hard work training pays off with a decent result.
Living in London, there’s a great scene of midweek evening criteriums*
and also a good selection of longer, road races* at the weekends,
usually a short drive or train ride away.
As a pretty competitive person, I always want to push myself further, go
bit faster and get better results.

Like most of the guys I race against, I have a full-time job and other
commitments which, ultimately, doesn’t leave an awful lot of time to
train.
On a good week, I can maybe get out on the bike for 10 hours, but on a
busy week, sometimes I can <i>only</i> manage around 4.
What I’m getting at here is - I’m never going to be able to
significantly increase my fitness by ramping up my training
…it just isn’t going to happen.

The last point: Luck. I like to think I’m a pretty rational person,
so I’m not going to start hunting for 4 leaf clovers or pin my hopes on
a lucky pair of socks.
Anyway, in the end, over the course of a season the luck usually
balances out.

#### <i>Turning the Pedals</i>

This got me thinking about the remaining 20%. Tactics.
Knowing when to attack, which moves to follow - this can represent the
difference between getting in a successful breakaway and battling it
out for the win, or spending your energy unnecessarily early on in the
race and not having enough left in the tank to be competitive towards
the end.
It usually comes down to fine margins, and having a little bit more
power left than the other guys, or arriving at the line with less people
in the group, makes a huge difference to your chances of getting on the
podium or finishing mid-bunch.

So, now getting onto the point of this post.
British Cycling (BC) has a fairly useful website, which riders can use
to enter races, via the races event page.
On most BC event pages its possible to see the riders on the startlist
for a race.
Whilst its possible to manually click through the startlist to see who's
any good (by navigating to the riders results page), it's quite a
laborious task.

Going back to a bit of race tactics, knowing your opponents has a pretty
big benefit.
For example, it's better to follow an attack from somebody who
consistently finishes in the top 10, compared to somebody who has raced
in 50+ races and yet to score a result ...in which case you'd probably
just be wasting energy.

#### <i>A Mid-Stage Breather</i>

Right so, what have we deduced so far.
Realistically, I’m not going to get significantly any fitter - or if I
do, I may have to completely give up my social life.
There’s information I can leverage from the BC website that can help me
with some race tactics, but the current way to do so is very slow.
However, this is a relatively easy problem for a python program to
solve.

#### <i>The Nitty Gritty </i>

Unfortunately, BC doesn’t provide a developer API, which is slightly
frustrating.
So plan B was to access the data via a custom web scraping script.
Its not the nicest way to access the data, but at least its structured
and can be repeated for each race.
So this was just a case of learning the structure of the BC website,
finding the corresponding html tags for the race entrants, and using the
href attribute for each entrant to navigate to their ‘points’ (or
results page).

Looping through the entrants list I created a Python 3.7+ ‘rider’
dataclass, assigning: the entrant name, club, and reading their points
table - as a pandas DataFrame - as attributes.
These ‘rider’ data classes I could then assign to a ‘field’ dataclass,
which is a list of the ‘rider’ dataclasses.
Cool, so thats all the scraping out the way.

```
race_field = Field(Rider(name_1, club_1, points_df_1), Rider(name_2, club_2, points_df_2), …)

```

The next step, was then figuring out what useful statistics I could
deduce from each riders points DataFrame.
From the riders ‘points’ table, I have a ‘result’ column and a ‘points’
column. e.g.


<b> Date</b> | <b>Event Title</b>	| <b>Type</b>	| <b>Cat</b> |<b>Position</b> |<b>Points</b>
10/5/19 | Amazing Race 1 | Closed Circuit | Regional C+ | 4 | 6
2/5/19 | Another Race  | Road | Regional A | 3 | 21
24/4/19 | Another Race 2  | Closed Circuit | Regional C+ | 12 | 0
16/4/19 | Another Race 3  | Closed Circuit | Regional C+ | 16 | 0

The results column is fairly self-explanatory, but the ‘points’ column
might need a bit more explaining.
Essentially, if you do well in a race you’re awarded points.
This is usually from 1st to 10th place for smaller races, and up 20th
place for bigger races.
The higher the category of race the more points are available i.e. in a
‘smaller’ race you can get 10 points for a win, whereas in a bigger race
you can get 60 points for a win.

So, from these two columns I decided on some ‘summary metrics’ that
would be useful to describe a rider:

* Total Wins (straightforward…)
* Top 10 Count (see how consistent a rider is at finishing at the
business end of a race)
* Total Points (straightforward…)
* Points per Race (This normalises the number of races a rider has
ridden. For example, if two guys have the same total point score, but
if one of them has raced in far fewer races - this would indicate they
are the stronger rider)

Since we have each riders points table stored as Pandas DataFrame,
calculating these metrics was fairly trivial - just some counting,
summing and/ or a combination of the two.

Right, so in essence that's it! For now I’m just writing out each riders
summary statistics to a csv, which allows me to get all the insight I
require.
To kick-off the program, I added a basic CLI, whereby you copy and paste
the race URL (all that's required for the scraper to go off and get the
riders points table) and input which year you’d like to get the stats
from.
This is useful, as if its the beginning of the season, not many people
will have done many, or even any, races at all - so in which case, its
probably more useful to have a look at the previous seasons results
instead. My output is essentially something like this:

<b> Name</b> | <b>Club</b>	| <b>Total Points</b>	| <b>Wins</b> |<b>Points per race</b> |<b>Top 10 Count</b>
Joe Bloggs | JB CC | 25 | 1 | 4.3 | 4
John Smith | Another CC  | 12 | 0 | 2.4 | 2
A Nother | Team CC  | 5 | 0 | 21.2 | 1


#### <i>The Final Sprint</i>

If I get more time, I could deploy this as web-app, with a neater
interface, but for now it does what I need it to do.
The summary stats created by the program give a quick and easy overview
of the other riders, saving a lot of time if I were to try and do some
research manually.
As to whether this helps out with the 20% tactics part of race success
…lets see how the season unfolds.


<br/>
<br/>
<br/>

***

<i>
### <u><i>Bike Lingo</i></u>

<b>Criterium</b> - A short (usually hour-ish long) race on an outdoor
circuit. The circuit can be anywhere between 1-3km in length, and the
race will do multiple laps of this. T
hese races are short and intense, with average speeds of usually 40kph+

<b>Road Race</b> - A longer race on ‘semi-closed’ roads, where marshalls
and escort motorbikes create a rolling road block.
These are usually anywhere between 80-150km in length, taking place on
‘normal’ roads, consisting of laps of a circuit 10-20km in length.
</i>

### Links

[Domestique Github repo](https://github.com/jcalcutt/domestique)
