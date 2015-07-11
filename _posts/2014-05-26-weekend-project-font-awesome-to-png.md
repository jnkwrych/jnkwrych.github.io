---
layout: post
title: Weekend project - fa2png.io (Font Awesome to PNG Online Generator)
---

This weekend my good friend [stephan](http://twitter.com/minddust) and I decided to start a weekend project. We've done a lot of coding together in the last two - three years. Mostly for the startup http://particule.me, where we worked together for the last two years. After working on this quite large project we felt like I'd be fun to do a small side project. So we decided to build a small app we would use ourselves.


## The Goal of the Project
The first an foremost goal was that the project should be fun and solvable in a short period of time (8h - 24h in our case). Meaning for us working with the languages and tools we know best to work efficiently and quickly. In our case Python, JavaScipt and HTML5. So a web app it is. The project should also deliver a value to its users so we tackled a problem we had our selves in the past.


## The Problem
Since I don't like working with Photoshop I have come to love icon fonts for my web and mobile apps. I use [Font Awesome](http://fontawesome.io/) all the time and it still amazes me how quickly you can build expressive UIs with just a few lines of html and css. But sometimes you need a real pixel image. For instance if you're doing a presentation or need a quick and dirty favicon.

In the past I used a python script developed by [odyniec](https://github.com/odyniec/font-awesome-to-png) to generate pngs from Font Awesome icons in custom sizes and colors. But you need PIL or Pillow for that and if you don't have one of those image manipulation libraries pre-installed quickly generating a small image can become an hour long odyssey.


## The Solution
In our day job and for bigger projects Django is our go to framework. It offers everything you need for a "normal" web app. Recently I read a lot about that frameworks like Django and Rails are too big and monolithic but I don't agree with that. For a lot of web apps the batteries included approach of Django and Rails are great. BUT since we read a lot about [Flask](http://flask.pocoo.org/) and used it successfully for a small web scraping script we thought it'd was perfect for our small Font Awesome generator app. For the frontend we used Bootstrap because we know the syntax inside and out and it makes development extremely fast. It may not be the most creative choice but it let us iterate much quicker. For the hosting we used [DigitalOcean](https://www.digitalocean.com/?refcode=db5b7d57ea35) (affiliate link) for the first time. So far DigitalOcean is pretty nice. We provisioned the servers with [ansible](http://www.ansible.com/). I used a slightly modified version of my [ansible deploy script for Django](https://github.com/j7nn7k/ansible-django-deploy) to provision the app's VM.


<img src="/img/fa2png-screenshot.png" class="img-responsive" alt="Font Awesome to png online generator">


## Conclusion
The implementation went pretty well and we were able to get the first version going after some hours. Today I deployed the code to our DigitalOcean VM. We're curious how the server will keep up.

All in all the project was a big success. It was fun, motivational and we were able to give something back to the community.

[Check the app here](http://fa2png.io/) and tell us what you think
