---
layout: post
---

This site is now also part of the [jekyll](http://jekyllrb.com) club. Like [many](https://github.com/jekyll/jekyll/wiki/Sites),
[many](http://paulstamatiou.com/how-to-wordpress-to-jekyll/), [many](http://tyler.io/2011/08/switching-from-wordpress-to-jekyll/)
folks before I also switched to jekyll today. I made the move because writing articles with my old setup was too tiresome. I wrote posts in plain html
which is fine but I had to copy the whole header and footer markup over every time I made a new post. Plus I had to
manually setup the folder structure.

I also thought about switching to [django](http://www.djangoproject.com/) or [wordpress](http://www.wordpress.org),
but static sites are more robust and way easier to maintain.

Jekyll sites can be hosted on GitHub's infrastructure which means, awesome performance at zero cost.

My friend and colleague [Stephan](http://www.minddust.com) switched to Jekyll a [few weeks ago](http://www.minddust.com/post/switch-to-github-pages/)
and gave me some tips on how to setup the site on [GitHub Pages](http://pages.github.com/).

Here's a http://blitz.io load test I ran today. I must say I'm pretty impressed with the performance. My website can
now handle over 400Mio requests a day at a stunning response time of 14MS. Awesome, no more holding back great articles to spare my server :-D

<img src="/img/jekyll-load-test.png" class="img-responsive" alt="My website could now handle over 400Mio requests a day
at a stunning response time of 14MS">

Here's the full report of the load test: https://www.blitz.io/report/9cefeda3e4c3a33e6f3ad511f33010aa

You can find the source code of this website in my [GitHub](https://github.com/j7nn7k/j7nn7k.github.io) repo.