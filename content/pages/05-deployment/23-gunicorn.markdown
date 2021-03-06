title: Green Unicorn (Gunicorn)
category: page
slug: green-unicorn-gunicorn
sortorder: 0523
toc: False
sidebartitle: Green Unicorn (Gunicorn)
meta: Green Unicorn (Gunicorn) is a Python WSGI server that runs Python web application code.


[Green Unicorn](http://gunicorn.org/), commonly shortened to "Gunicorn", 
is a [Web Server Gateway Interface (WSGI) server](/wsgi-servers.html)
implementation that is commonly used to run Python web applications.

<a href="http://gunicorn.org/" style="border: none;"><img src="/img/logos/gunicorn.jpg" width="100%" alt="Official Green Unicorn (Gunicorn) logo." class="technical-diagram"></a>


## Why is Gunicorn important?
Gunicorn is one of many WSGI server implementations, but it's particularly
important because it is a stable, commonly-used part of
[web app deployments](/deployment.html) that's powered some of the
largest Python-powered web applications in the world, such as 
[Instagram](http://instagram-engineering.tumblr.com/post/13649370142/what-powers-instagram-hundreds-of-instances).

Gunicorn implements the 
[PEP3333 WSGI server standard specification](https://www.python.org/dev/peps/pep-3333/)
so that it can run Python web applications that implement the application
interface. For example, if you write a web application with a 
[web framework](/web-frameworks.html) such as [Django](/django.html), 
[Flask](/flask.html) or [Bottle](/bottle.html), then your application 
implements the WSGI specification. 

<div class="well see-also">Gunicorn is an implementation of the <a href="/wsgi-servers.html">WSGI servers</a> concept. Learn how these pieces fit together in the <a href="/deployment.html">deployment</a> chapter or view the <a href="/table-of-contents.html">table of contents</a> for all topics.</div>


## How does Gunicorn know how to run my web app?
Gunicorn knows how to run a web application based on the hook between the 
WSGI server and the WSGI-compliant web app.

Here is an example of a typical Django web application and how it is run
by Gunicorn. We'll use the 
[django\_defaults](https://github.com/mattmakai/compare-python-web-frameworks/tree/master/django_defaults) 
as an example Django project. Within the [django\_defaults](https://github.com/mattmakai/compare-python-web-frameworks/tree/master/django_defaults/django_defaults) 
project subdirectory, there is a short [wsgi.py](https://github.com/mattmakai/compare-python-web-frameworks/blob/master/django_defaults/django_defaults/wsgi.py)
file with the following contents:

    """
    WSGI config for django_defaults project.

    It exposes the WSGI callable as a module-level variable named ``application``.

    For more information on this file, see
    https://docs.djangoproject.com/en/1.8/howto/deployment/wsgi/
    """

    import os

    from django.core.wsgi import get_wsgi_application

    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "django_defaults.settings")

    application = get_wsgi_application()

That `wsgi.py` file was generated by the `django-admin.py startproject` command
when the Django project was first created. Django exposes an `application` 
variable via that `wsgi.py` file so that a WSGI server can use `application` 
as a hook for running the web app. Here's how the situation looks visually:

<img src="/img/visuals/gunicorn-django-wsgi.png" alt="Gunicorn WSGI server invoking a Django WSGI application." width="100%" class="technical-diagram" />


## What's a "pre-fork" worker model?
Gunicorn is based on a pre-fork worker model, compared to a worker model
architecture. The pre-work worker model means that a master thread spins up
workers to handle requests but otherwise does not control how those workers
perform the request handling. Each worker is independent of the controller.


### Gunicorn resources
* There are three framework-specific posts on the 
  [Full Stack Python blog](/blog.html) for configuring Gunicorn for 
  development on Ubuntu: 
    1. [Setting up Python 3, Django and Gunicorn on Ubuntu 16.04 LTS](/blog/python-3-django-gunicorn-ubuntu-1604-xenial-xerus.html)
    1. [How to set up Python 3, Flask and Green Unicorn on Ubuntu 16.04 LTS](/blog/python-3-flask-green-unicorn-ubuntu-1604-xenial-xerus.html)
    1. [Configuring Python 3, Bottle and Gunicorn for Development on Ubuntu 16.04 LTS](/blog/python-3-bottle-gunicorn-ubuntu-1604-xenial-xerus.html)

* [gunicorn as your Django development server](https://vxlabs.com/2015/12/08/gunicorn-as-your-django-development-server/)
  is a short post with a few good tips on using Gunicorn for local
  application development.

* The [Full Stack Python Guide to Deployments](http://www.deploypython.com/)
  provides detailed step-by-step instructions for deploying Gunicorn as part
  of an entire Python web application deployment.

* [Flask on Nginx and Gunicorn](http://prakhar.me/articles/flask-on-nginx-and-gunicorn/)
  combines the [Nginx web server](/nginx.html) with Gunicorn in a deployment
  to serve up a Flask application.

* The answers to the question "[what's the best practice for running Django with Gunicorn?](http://stackoverflow.com/questions/16857955/running-django-with-gunicorn-best-practice)"
  provide some nuance for how Gunicorn should be invokin the callable 
  application variable provided by Django within a deployment.

* [How to Install Django with Gunicorn and Nginx on FreeBSD 10.2](http://linoxide.com/linux-how-to/install-django-gunicorn-nginx-freebsd-10-2/)
  is a tutorial for FreeBSD, which is not often used in walkthroughs compared 
  to the frequency that Ubuntu and CentOS tutorials appear.

* [How to make a Scalable Python Web App using Flask, Gunicorn, NGINX on Ubuntu 14.04](http://www.philchen.com/2015/08/08/how-to-make-a-scalable-python-web-app-using-flask-and-gunicorn-nginx-on-ubuntu-14-04)
  and
  [Deploy a Flask App on Ubuntu](https://github.com/defshine/flaskblog/wiki/Deploy-Flask-App-on-Ubuntu(Virtualenv-Gunicorn-Nginx-Supervisor))
  both provide steps for setting up a Flask web app using Gunicorn. There 
  isn't much explanation provided with each tutorial but they can still be
  good concise references in case you're having issues with Ubuntu.

* [Deploying a Flask Site Using Nginx, Gunicorn, Supervisor and Virtualenv on Ubuntu](http://alexandersimoes.com/hints/2015/10/28/deploying-flask-with-nginx-gunicorn-supervisor-virtualenv-on-ubuntu.html)
  is a similar tutorial to the previous two links. It provides some good
  screenshots along the way with what to expect while you are configuring
  the deployment server.

* The [Django](https://docs.djangoproject.com/en/1.9/howto/deployment/wsgi/gunicorn/) 
  and [Flask](http://flask.pocoo.org/docs/latest/deploying/wsgi-standalone/)
  documentation each contain instructions for deploying the respective
  frameworks with Gunicorn.

* [Set up Django, Nginx and Gunicorn in a Virtualenv controled by Supervisor](https://gist.github.com/Atem18/4696071)
  is a GitHub Gist with some great explanations for why we're setting up
  virtualenv and what to watch out for while you're doing the deployment.

* The 
  [Gunicorn design document](http://docs.gunicorn.org/en/stable/design.html)
  is worth reading because it describes the server model and gives some 
  context on how to choose the number of workers for your execution 
  environment.

* [Configuring Gunicorn for containers](https://pythonspeed.com/articles/gunicorn-in-docker/)
  explains how to avoid excessive slowness in your [Docker](/docker.html)
  containers that run Gunicorn workers, as well as some tips on
  proper logging.
