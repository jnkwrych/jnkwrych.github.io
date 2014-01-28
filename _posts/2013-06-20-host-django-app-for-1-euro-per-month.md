---
layout: post
title: Host your Django App for 1€/month
---

This post describes how you can deploy your Django application on [Uberspace](http://www.uberspace.de). Uberspace is
an awesome and somewhat unconventional hoster. It’s shared hosting but with a full-fledged Cent-OS command line
interface offering everything a normal vps does except root access. But if you need software installed that
needs root access the awesome customer service will help you out instantly. PLUS: They allow you to pay as much
as you want (but min. 1€ per month). We use Uberspace primarily for demos and small websites.

We also describe a convenient deploy scenario with git post-receive hooks to do super simple deployments without
much overhead.

Since Uberspace has some technical limitations the setup is a bit different than on a conventional vps or root
server (e.g. fcgi instead of wsgi). For our own reference and for people looking for a step by step tutorial
that works without much cross-reading we decided to post our setup.

NOTE: This is how we deploy our apps on Uberspace in detail so it is somewhat opinionated. Feedback is much
welcome. Just ping Jannik on Twitter ([@jnk_wyrich](https://twitter.com/jnk_wyrch))
or discuss on [Hacker News](https://news.ycombinator.com/item?id=6768241) or [Reddit](http://www.reddit.com/r/django/comments/1r26t0/host_your_django_app_for_1month/).

## Prerequisites

We gonna keep it brief but show all the necessary steps. Check the [sources section](#sources) for
more background info. We assume you already have your Django project in a Git repo.

**Important**: Please add [South](http://south.aeracode.org/) to your
requirements file. Since it’s part of our deploy script.

## The Django settings file

We just set the static directories and database configuration. The rest of the django settings are up to you.

```python
STATIC_ROOT = '/home/<YOUR_UBERSPACE_USER>/html/static'
MEDIA_ROOT = '/home/<YOUR_UBERSPACE_USER>/html/uploads'

STATIC_URL = '/static/'
MEDIA_URL = '/uploads/'

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': '<YOUR_UBERSPACE_USER>',
        'USER': '<YOUR_UBERSPACE_USER>',
        'PASSWORD': '<YOUR_MYSQL_PW>', # find it in the following file: ~/.my.cnf
        'HOST': '/var/lib/mysql/mysql.sock',
        'PORT': '', # Set to empty string for default.
    }
}
```

## MySQL

Your Uberspace provides MySQL databases for you to use.

You can access your database data under `https://adminer.<YOUR_SERVER_NAME>.uberspace.de/`. The
primary database name and user is the name of your Uberspace user. Your MySQL password is stored in the file
`~/.my.cnf` on your Uberspace.

## Python Setup

To get Python and Django on your Uberspace running you need to do the following steps (described in the official
[Uberspace Wiki](http://uberspace.de/dokuwiki/cool:django))

To be able to install Python modules:

```
mkdir -p ~/bin ~/lib/python2.7
```

Install flup for fast-cgi. (WSGI does not work on Uberspace [see Uberspace
Wiki](http://uberspace.de/dokuwiki/cool:django#deployment_mit_fastcgi)).

```
pip-2.7 install flup
```

## How to get your project running

Now that the basic environment for our Django project is done, we describe how to setup the static and media
paths and how to deploy your project via git.

First you create a directory structure to store the application in. We made the structure up ourselves. It’s just
our recommendation. You can obviously use other paths and dir names.

```bash
mkdir -p ~/data/apps
git init --bare ~/data/git/<YOUR_PROJECTNAME>.git
vi ~/data/git/<YOUR_PROJECTNAME>.git/hooks/post-receive
# Update the post-receive hooks permissions
chmod 755 ~/data/git/<YOUR_PROJECTNAME>.git/hooks/post-receive
```

Paste the following into the file `~/data/git/<YOUR_PROJECTNAME>.git/hooks/post-receive`.

Fill in your data where <INDICATED>.

```bash
#!/bin/sh

USER=<YOUR_UBERSPACE_USER>
# e.g. PROJECT_NAME=myproject
PROJECT_NAME=<YOUR_PROJECTNAME>
PROJECT_PATH=/home/$USER/data/apps/$PROJECT_NAME
# e.g. STATIC_DIR=$PROJECT_PATH/static
STATIC_DIR=$PROJECT_PATH/<PATH_TO_YOUR_DJANGO_STATIC_DIR>
# e.g. REQUIREMENTS_FILE=$PROJECT_PATH/requirements.txt
REQUIREMENTS_FILE=$PROJECT_PATH/<PATH_TO_YOUR_PIP_REQUIREMENTS_FILE>
# e.g. MANAGE_PY=$PROJECT_PATH/manage.py
MANAGE_PY=$PROJECT_PATH/<PATH_TO_YOUR_DJANGO_MANAGE_PY>

killall -u $USER python2.7

if [ ! -d /home/$USER/data/apps/$PROJECT_NAME/.git ]
then
    git clone /home/$USER/data/git/$PROJECT_NAME.git /home/$USER/data/apps/$PROJECT_NAME
        pip-2.7 install -r $REQUIREMENTS_FILE
    python2.7 $MANAGE_PY syncdb --noinput
else
    unset GIT_DIR
    cd $PROJECT_PATH && git checkout -- .
    cd $PROJECT_PATH && git pull
    pip-2.7 install -r $REQUIREMENTS_FILE
fi

# South required to do DB migrations
python2.7 $MANAGE_PY migrate
cd /var/www/virtual/$USER/html/static && rm -rf * .[^.]* ..?*
mkdir -p /var/www/virtual/$USER/html/static
python2.7 $MANAGE_PY collectstatic --noinput
mv $STATIC_DIR/* /var/www/virtual/$USER/html/static/
killall -u $USER python2.7

exit
```

Add a fast CGI file.

```bash
vi ~/fcgi-bin/<YOUR_PROJECTNAME>.fcgi
chmod 755 ~/fcgi-bin/<YOUR_PROJECTNAME>.fcgi
```

With the following configuration.

```python
#!/usr/bin/env python2.7
import os
import sys

# Add a custom Python path.
sys.path.insert(0, '/home/<YOUR_UBERSPACE_USER>/data/apps/')
# This path links to the dir where your manage.py is located
sys.path.insert(0, '/home/<YOUR_UBERSPACE_USER>/data/apps/<YOUR_PROJECTNAME>/')

# Set the DJANGO_SETTINGS_MODULE environment variable.
# The location depends on your project structure
os.environ['DJANGO_SETTINGS_MODULE'] = '<YOUR_PROJECTNAME>.settings'

from django.core.servers.fastcgi import runfastcgi
runfastcgi(method='threaded', daemonize='false')
```

Setup a .htaccess in your Uberspace’s public directory. Find it here `~/html/.htaccess`.


```bash
RewriteEngine on

RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)$ /fcgi-bin/<YOUR_PROJECTNAME>.fcgi/$1 [QSA,L]
```

##  Deploying your Django app from your local machine

Now that your server is setup all that’s left is to deploy your application code to the server.

To do so you need to add a new git remote to the local git repository of your Django project.

```bash
git remote add uberspace ssh://<YOUR_UBERSPACE_USER>@<YOUR_SERVER_NAME>.uberspace.de/home/<YOUR_UBERSPACE_USER>/data/git/<YOUR_PROJECTNAME>
```

Now every time you push to the `uberspace` remote the post-receive hook we set up before gets
triggered and does all the steps needed to (re)deploy your Django application. Stuff like pull the application
code, update the requirements, migrate your database, updating your static files, etc.

## Conclusion

The setup we described above is a cheap, solid and convenient way to run Django applications. Perfect for small
projects, experiments or [minimum
    viable products](http://en.wikipedia.org/wiki/Minimum_viable_product) (MVP). Especially when coding experiments or MVPs you might want to update your
application pretty frequently without the overhead of manual deploys. With this setup all you need to do is
commit your new code to the `uberspace` git remote and it’s live.

It’s most certainly not suited for applications with high traffic or big data needs. Uberspace limits the quota
to 10GB and you share a server with other users. But performance-wise Uberspace’s servers are pretty good. Most
likely way better than most shared hosters. We did some load tests with [blitz.io](http://www.blitz.io/) and the
results were solid. We might write blog post about it later.

Uberspace's website is only available in German. Visit this link to get a translated version: [Uberspace via Google Translate](http://translate.google.com/translate?hl=en&sl=auto&tl=en&u=https%3A%2F%2Fuberspace.de%2F)

If you have questions or want to discuss leave a comment on [Hacker News](https://news.ycombinator.com/item?id=6768241) or [Reddit](http://www.reddit.com/r/django/comments/1r26t0/host_your_django_app_for_1month/).

Or ping me on Twitter ([@jnk_wyrich](http://twitter.com/jnk_wyrch))

This is a cross post on [here](http://jannikweyrich.com/blog/2013/11/20/deploy-django-app-for-1-euro-per-month.html) and [Particulates's
blog](http://tech.particulate.me/django/2013/11/20/deploy-django-app-for-1-euro-per-month/).

## <a name="sources" id="sources"></a>Sources

[http://uberspace.de/dokuwiki/cool:django](http://uberspace.de/dokuwiki/cool:django)

[http://uberspace.de/dokuwiki/development:python](http://uberspace.de/dokuwiki/development:python)

[http://uberspace.de/dokuwiki/database:mysql](http://uberspace.de/dokuwiki/database:mysql)
