---
layout: post
title: "A Note On This Site"
author: "Jordan Calcutt"
categories: journal
tags: [html, css, jekyll, git, meta, blog]
image: spools.jpg
---



I've built this site using <i>Jekyll</i> and <i>Github pages</i>.
At a high level, Jekyll takes care of the content-management -
it is a static site generator - and Github hosts the site.
This is great as it allows me <i>full</i> control of many things,
including all the HTML and CSS code. I can also run and test any updates
I make locally, before pushing my updates to Github - where the site is
hosted for the outside world to view. Here, I'll briefly describe a few
of the intricacies involved in setting up, modifying and maintaining
this site.


## Why Jekyll and Github Pages?

In short, this combination ticks all my boxes.

I don't have the rigid, non-customizable interface of something like a
Wordpress site. Also, on the contrary, I don't have to write all the
HTML and CSS code from scratch myself, and then find and pay a
subscription for a host server and domain name.

The Jekyll framework, built in Ruby, provides a template, but ultimately
I'm responsible for the entire format of the site.
I have full control of the code, but a lot of the labour-intensive work
has been taken care of.
Hosting the site with Github pages provides me with a domain name
<i>jcalcutt.github.io</i> and my site is served form here.
An added bonus of hosting on Github is that version control and updating
my site is a breeze too.


## Setting Up

Assuming you already have a Github account, there's a few ways to set up
a Jekyll site. These are the steps I took to set up:

- Browsed [Jekyll Themes](http://jekyllthemes.org/) and forked a repository
to my Github account
- Change the name of this forked repository, in this case to
'USERNAME.github.io' ...<i>more info on naming conventions [here](https://help.github.com/articles/user-organization-and-project-pages/)
- Edit the _config.yml (and _data/settings.yml) files as necessary
- In Github settings, ensured Github pages is being served from the
correct branch - 'origin/master' in my case.

Now, in theory, the site is all ready to go.
I could just make changes to
some configuration settings and add my own markdown files to the <i>'_posts'</i>
directory.
However, this wouldn't be very efficient. I want to be able
to view and test any changes I make without immediately pushing them to
the 'production' version i.e. what the outside world see when they visit
jcalcutt.github.io.

So, next I cloned my repository to a local directory:
```
~/site_directory $ git clone git@github.com:jcalcutt/jcalcutt.github.io.git
```
I already had Ruby and RubyGems installed, so I just needed
to install Jekyll.
```
$ gem install jekyll
```
With Jekyll installed, I'm able to run a local, jekyll development
server at <i>http://localhost:4000/</i> with:
```
~/site_directory $ jekyll serve
```
Therefore, I'm able to test and view any changes I make to the HTML
or preview any new posts I make before committing any changes to the
'origin/master' branch in the website repo.

The only thing to do now is to add new content to the '_posts'
directory in the form of markdown files. At the top of each .md file I
need a YAML front matter block, e.g.

```
---
layout: post
title: "A Note On This Site"
author: "Jordan Calcutt"
categories: journal
tags: [html, css, jekyll, git]
image: spools.jpg
---
```
This essentially interacts with all my pre-configured settings, taking
care of all the post formatting and many other features.
Below this block I can write all the actual post content in markdown.
Finally, it's just a case of launching a jekyll server to test if
the markdown has been converted correctly and then committing updates
to my Github repo.
Easy!

### Links

[More info on Jekyll](https://jekyllrb.com/)

[More info on hosting with Github pages](https://pages.github.com/)

[Another guide to setting up with Jekyll and Github pages](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)




