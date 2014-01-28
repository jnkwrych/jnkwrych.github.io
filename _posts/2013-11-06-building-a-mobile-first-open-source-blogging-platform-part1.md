---
layout: post
title: A mobile-first open source blogging platform | part 1
---

This is the first post in a series of posts about a new project I’m planning to pursue together with my
friend and collegue [Stephan](http://www.minddust.com/) and hopefully many open
source devs out there.

## tl;dr

We’re building an open source **mobile first** blogging platform. Because existing blogging and
posting tools are not good enough. The first stable version
shall be due on February 1st 2014. Why reinvent the wheel, you ask? Read further. If you rather wanna see some
code,

[Check our github repo](https://github.com/j7nn7k/flashpacker)

## Why reinvent the wheel?

I know, there are a bazillion blogging tools out there! Why not use twitter, tumblr, medium or facebook? These
platforms have pretty good apps from which you can post short “articles" pretty easily. Why not use them?

*   Well, those tools are great for certain use cases, but they’re proprietary platforms which don’t offer much
    customization options like custom layouts, custom post types or custom domains and stuff like that. Also you
    have no say as to where and how long your data is stored.

But what about, wordpress? Wordpress offers customization options and even self hosting.

*   While wordpress is great for traditional blogging it hasn’t been created for mobile blogging or short posts
    like twitter, tumblr or medium. One of the strongest features of wordpress is also one of it’s weakest -
    it’s customization options. You need design and coding skills to customize wordpress’ design and layout.
    Also it doesn't provide much of a
    frame to make short, quick posts. For instance, I just wanna snap a picture of a nice bamboo hat hostel in
    the tropical jungle of thailand, add gps data, a short comment and a rating from 1-5. When I was on my
    trips, I often wanted to do posts like that. But I never did. Either because the website was slow, I needed
    to
    fiddle with layouts, images, text formatting and what have you. **Blogging should be simpler!**

We want to combine the ease of use of proprietary solutions like twitter, medium or facebook and the
customization options of wordpress.
People should be able to quickly and easily blog on the go and still have their own custom layouts, domain and
even servers if they want to. To make that happen we want to build cool and easy to use apps. One important
feature is that it should be easy to write posts when there is no internet connection and upload them when you
get back online. Everybody should be able to decide where and how their data is stored without much friction!
Don’t get me wrong we don’t want to build a jack of all trades. We wanna build an open source mobile-first
blogging platform for special use cases. For people on the go, traveling, getting around, working hard, with
little time but the desire to share cool stuff with the world.

## Our vision

> Flashpacker is a mobile-first blogging platform that is build for people on the go who don’t want to be
>             stuck in some kind of ecosystem but want simple and easy to use apps to post their content online.

## Background info

I came up with this idea because I have the need for such a platform but none of the existing tools fits my
needs. I love to travel and being on the road. I'd like to blog about that kind of stuff from my [smartphone](http://www.jannikweyrich.com/#tech-i-use). I also like to watch movies and series. I’d
like to have a blog where I can score, write short reviews and link to sites like [IMDB](http://imdb.com)
quickly and easily while I’m still at the movies or where ever I am.

> I’m sure a lot of people have the same needs when they blog about their hobbies and favorite activities.

Blogs are an awesome medium for everyone to spread valuable information across the internet so
other people can consume this information. The best thing about blogs is the vast variety of topics. From
serious, to professional to silly. Doesn’t matter. For every blog post out there there is at
least one person in the world that values that information and gets something positive out of it. Sometimes
it’s only the author himself but even that is a great reason to blog.

**My main problem about blogging: I do not blog enough.** I wanna change that. Not for the sake of
marketing myself or a company. While that is a
pretty important and legit reason to blog, I wanna blog more because I think stuff that I experienced can come
in handy for others and even for myself in the future.

Let me give you an example. I traveled to some pretty nice but unknown places in the Philippines, Brazil and
Thailand which
I really liked. Beautiful beaches, cool places to hang out, cool people, crazy cheap accommodations and so
on. I wanna let people know that these nice places exist so they can explore them for themselves. I wanted to
blog about that stuff. But I didn’t. And not because I was lazy. I was on vacation. I was motivated. I had
plenty of time and enthusiasm to write stuff
down, upload photos and post them on the internet. Why didn’t I?

*   Because it wasn’t easy enough.

Crappy internet connections, no computer nearby, not enough content to fill a whole blog page and so on.
Blogging shouldn’t be a pain. It should be fun and easy. So I thought before I spend half of my vacation trying
to upload something to my blog over a crappy mobile app or trying to find an internet cafe. I’d rather let it go
and enjoy what’s going on around me.

> ## My first “travel blog post"
>
> <img src="/img/kao-sok-blog-2013-11-06.jpg" class="img-responsive" alt="Kao Sok, National Park, Thailand">
>
> Kao Sok, National Park, Thailand
>
> So there you go. The first picture I posted from one of my awesome trips! We spend some nights in those
> bamboo hats floating on a lake in the middle of nowhere with no electricity in the Kao Sok, National Park,
> Thailand.

I have so much more to blog about. But even though I’m
sitting in my office right now and have a nice setup here this wasn’t too easy to post. Maybe because
I’m writing plain html in Sublime Text, but anyways... ;-) I’d like to have a blog that I can post to whenever
I want from wherever I want! An the most important thing: It has to be fast, easy and the way I want it to be!
Because otherwise I won’t blog at all or much less that I’d like to.

That’s why we are building [flashpacker](https://github.com/j7nn7k/flashpacker).

## What do we have so far?

We have one specific deadline. I will spend 3 weeks in South Africa and the Seychelles in February 2014. Until
that date there
should be a solid, usable version of the app. Even though it’s a side project that should be doable.

We also have a plan for the architecture of the blogging platform. I'll keep this part brief because I'll go
more in depth in my next post. As the platform’s server backend we plan to develop a django app backed by the
awesome [django rest framework](http://django-rest-framework.org/). We will
distribute the flashpacker server backend app via PyPI so it can be included into any other django project. The
flashpacker django app will be a pure RESTful api
without any web frontend (except the django admin). This way we can develop one single backend for
Android, iOS, web apps or whatever other platform. We will also start developing open source Android and iOS
apps which will be the only applications for posting on the blogging platform at first. But since we develop a
RESTful api and all the code will be open source it is up to everyone to develop client apps to
connect to the django server’s RESTful api.

## What are we going to do next?

*   Publish the next blog post which will be about the architecture of the app.
*   Write some code for the django server backend.

## Like the idea? How you can help.

You like our idea and wanna contribute? You can help us by writing code or opening issues for our Django,
Android or iOS apps.

[Check our github repo](https://github.com/j7nn7k/flashpacker)

You can also help by spreading the word on
Twitter, Facebook, Reddit, Hacker News
