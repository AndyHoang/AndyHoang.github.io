---
layout: post
title: "60 minutes setting up my first blog!"
date: "2017-03-19 00:33:52 +0700"
---


## Getting started
As far as I remembered, I created a blog 2 year ago, when Github "first" marketing about its feature: github.io blog. I was excited and try it out right away. But umm... it was nothing more than one line __Hello world__ in the index.html file.

Ok, let skip that. Let create a brand-new one.

## Static blog

I've heard about this term before but haven't really spend time one it. After digging around 30 mins (sorry I lied about 60 mins stuff), I figured out some frameworks that I could use including in Haskell (language I've recently interested in).

But hey, we need server for that right?... Ok, go back with thing provide from Github. Well, it's Jekyll.

Let try!

## Jekyll
Jekyll, in __Ruby__, okey...

Googled a little bit more and __bump__! Everything is settled.

Yeah, like super easy peasy. Just copy all the files (our just fork) the [jekyll-now](https://github.com/barryclark/jekyll-now) repo and you're done!

I also installed `bundle` and `gem` for ruby so that I can see my post in my local machine. Of course - reviewing typos, actually I know my post still have typos and grammar errors in only 100 words above, and... below.

## Custom domain
This one is hard. I'm using Amazon route 53 for my domain. Not sure it is harder compare to others though. Well, in my defense, it is just my __second time__ buying a domain! (The first one was ~~jav...something.net~~, oh I forgot....)

After 30 mins going nowhere, google strike me again, I found this [Tutorial](http://sophiafeng.com/technical/2015/02/12/setting-up-custom-domain-name-with-github-pages-and-amazon-route-53/). It's fit perfectly with my case: Domain from Amazon and using github.io blog. Thanks bro!!

__Nice!__

## First post!
Well if you're planning to use Jekyll with Github and local review, I suggest you add those to `.gitignore`
```
/_site/
*/_site/
```
Those was generated file, Github can do it for us.

## Thing left?

- About me???
- Write Vietnamese in vim
- Tryout themes for this boring blog
- Add comment section. This [Tutorial](https://mademistakes.com/articles/jekyll-static-comments/) look promising!
- Make it work with https
