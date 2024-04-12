---
layout: post
title: A Dockerfile to Run Playwright on Digital Ocean App Platform
description: Discover how to effortlessly deploy Playwright on Digital Ocean's App Platform through our expert guide. Learn to navigate dependency and root access issues for browser automation projects. Ideal for developers looking for a hassle-free deployment without the complexity of managing a VPS. Access our proven script for Digital Ocean to fast-track your Playwright project's success. Perfect for enhancing your web automation tasks.
---

I've been a [fan](https://jannikweyrich.com/blog/2016/09/24/how-to-setup-united-domains-with-digital-ocean.html) of Digital Ocean for a couple of years now, and recently, I wanted to explore their fully managed [Application Platform](https://www.digitalocean.com/products/app-platform) offering, which is similar to Heroku.

Deploying a simple Node.js API was as easy as linking my GitHub Project Repo and letting the Application Platform handle the configuration and deployment automatically.

However, I encountered problems when I wanted to deploy a project running the browser automation framework, [Playwright](https://playwright.dev/). Playwright has some dependencies, mostly related to installing browser components (Chromium, in my case), which require root access. Unfortunately, this is not possible on the Digital Ocean Application Platform.

Since I did not want to deploy and manage a full-blown VPS, or "Droplet" as Digital Ocean calls them, I looked for ways to make Playwright run anyway. Fortunately, I [found a threat by someone](https://gist.github.com/BrianVia/5826f1057e2d6c07836a2c3fc5ce4104) having a similar issue related to Puppeteer, which is very similar to Playwright. So, I was confident that the App Platform was capable of running Playwright as well.

## The solution

Here is the script I used to deploy my Playwright Node app to Digital Ocean's App Platform. Bear in mind that I have not tested the script in a production environment, but I hope it can help you get your project up and running more quickly.

<script src="https://gist.github.com/jnkwrych/c647d8653515dc676f7369507795b9d8.js"></script>
