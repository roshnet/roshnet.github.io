---
layout: post
title:  "Getting started with Falcon"
comments: true
description: "tutorial on rest api in python falcon, rest api in falcon python,
get started with falcon python"
date:   2020-01-25 16:07:43 +0530
---

<style>
/* Plain text formatting */
p, li {color: #164669c2; font-size: 110%;}
h1, h2, h3, h4 {color: grey;}
blockquote {font-family: monospace; font-size: 15px;}

/* Code formatting */
.highlighter-rouge .highlight {
    /*background-color: #236291;*/
    background-color: #123752;
}
code {
    background-color: #123752;
    color: #25cee0;
}
.highlight .nn {color: #adeab5;}

/* Custom classes */
.note {
    font-size: 85%;
    padding: 10px 20px;
}
.note-dark {
    color: white;
    background-color: rgb(31, 98, 179);
    /* background-color: rgb(1, 10, 138); */
}
.note-light {
    background-color: rgb(185, 229, 247);
}
.highlighted {
    /* padding: 10px 20px; */
    color: #0fc;
}
</style>

<hr>
<br>

# Falcon

If you're a web developer, you probably have heard about a number of web
frameworks out there, or might have worked on applications built with them.
A quite awesome one of them is Flask. It's a pretty straight-forward,
_help-yourself_ kind framework.

And, you've heard about Django too, which is one hell of a hefty framework,
owing to it's batteries-included approach.

Now, where does Falcon fit in on the list?

On grounds of speed, I'd say it leads. Moreover, Falcon's source states:

> ... in a conflict between saving the developer a few keystrokes and saving a few
microseconds to serve a request, Falcon is strongly biased towards the latter.

So, if you plan on getting wheels-up quick, I'd suggest avoiding Falcon-type
minimalistic frameworks.
But, if you're the Flask-kind, and into the REST world, it's worth giving out a
try.


## Intuition

### Requests and Reponses

To access anything on the web, a client (users, like us) needs to send a
**request** to a server, which then handles the request and returns a
**response** based on the logic it is programmed for. With Falcon, we create
the server side of this relation - the part which handles requests and returns
responses.


### Resources

There is a common term, called an "endpoint" - a URL slug of an API, used to
handle traffic for different parts of the website (or an API, for that matter).

A nice example follows: "https://google.com/search" will render the page for
the search engine, whereas "https://google.com/mail" will take me to Gmail.
**'/search'** and **'/mail'** are basically the _endpoints_ for the same server that
hosts *https://google.com*.

In Falcon, we think in terms of resources. Let's think of a website which
lets you buy books - it needs to have some logical entities associated with it. For this
example, we have _users_, the _books_, _purchases_, and so on. These are entities
which have specific logical attributes. Like, _users_ can log in or log out - books can't.
Or, books can be sold or have discount, users can't.
This gives us a vague sketch that our *users* resource must have some kind of login
functionality associated with it, and our _books_ must have some code which governs their
sales.

Hence, we always think in terms of resources, and create our application with these
resources as building blocks of our application.

A `Resource` class essentially allows us to write code to handle the request
when a specified endpoint is hit. Hence, they can also be known as responder
classes.

The resource class needs to have specific methods for each "type" of request.
Here, the "type" is the HTTP verb under which the request is operating.
As an example, when submitting an HTML form, the request is of type "POST"
(usually specified as the "method" attribute of &lt;form&gt;). Here, we
classify our resource methods according to what request type it expects.

The methods of the resource class are named `on_get()`, `on_post()`,
`on_put()`, and so on. These are also referred to as the `on_*` methods.

A typical Falcon resource class looks like this:
```py
class ImageResource():
    """Class for uploading and fetching images"""

    def on_get(self, req, resp):
        """Fetch all images"""
        # Code to fetch a list of all images #
        pass

    def on_post(self, req, resp):
        """Upload an image"""
        # Code to upload an image #
        pass

```

It's a common practice to suffix response class' names in `...Resource` (like
LoginResource, ImageResource, et cetera).
This makes it easier to distinguish them from other custom classes we might have
declared in our project.

<p class="note note-dark">
It's a misconception that requests with query parameters (URL embedded
parameters) are GET, and the ones with a request body are POST.
Both types can contain information in both ways. So, a POST can have query params,
and a GET can also have a request body.
</p>


## Building a login endpoint

In order to adhere to the basics, and make Falcon occupy most of our attention,
things need to be kept as simple as possible. Hence, we stick to creating a
single endpoint which tells us whether or not to log a user in, based on the
supplied credentials.

For the sake of simplicity, we won't bother with how those are stored/fetched
from a database. We'll use pre-defined variables to compare against for that
matter.

### Directory structure

The first thing to decide is the directory structure. For small projects,
we can follow the old school _all-code-one-file_ approach, but it definitely
isn't the way to go if we're talking APIs with more than, I don't know,
5, maybe 10 endpoints.

Adding and debugging features becomes helluva lot easier when the source code
is organized.

For me, the directory structure that works best is as follows:

```
ProjectRoot/
|---index.py
|---api/
|     |--- __init__.py
|     |--- middlewares.py
|     |--- routes/
|          |--- auth/
|          |    |--- signup.py
|          |    |--- login.py
|          |--- orders/
|          |    |--- get_all.py
|          |    |--- create.py
|          |--- customers/
|               |--- public/
|               |    |--- viewall.py
|               |--- members/
|                    |--- update_profile.py
|                    |--- get_transaction_history.py
|--- .gitignore
|--- Procfile
|--- README.md
|--- venv
|--- ... and other project-level files one might have
```

The above naming should give an idea of how managing stuff becomes easy when
things are as organized as above. Say, you're getting unexpected behavior
when you view all transactions by a user - you guessed it - you focus your
efforts _only_ on the `get_transaction_history.py` module, and follow the
traceback, _ignoring every other module in the entire code base_ (assuming
an ideal case here). This can prove a mighty egde in debugging, since you're
well aware of where to start.

This is an imaginary but similar directory structure I've made up for the sake
of this post. To peek into a live production API, please head
[here](https://github.com/roshnet/blogstate-api).


## Alright, let's code!

We start by creating our entry point file - **index.py**.

This file exposes our yet-to-come `app` variable, to the WSGI server that
serves our app. The reason it's like this because that's how a production
server serves our app when we deploy. More on that later.

#### 1. Create a new file **index.py**

```py
from api import app
```
Let's now create this **app** variable, in **api/__init__.py**.

#### 2. Create the **api/** directory
```
$ mkdir api/
```

#### 3. Create **__init__.py** inside **api/**
```py
import falcon

app = falcon.API()

from api.routes.auth import login
```
This might seem crazy - using things before defining them, but it actually
does one good thing - you are never distracted because you're aware of what
to do next!

Notice in #3, we haven't placed the import for `routes` at the top, and many
linters might shout at you for this. But, it's necessary as the routes will
need access to the `app` variable, which therefore has to be defined before
calling the routes.

<p class="note note-dark">
If you're sick of the linter's warnings, add `# noqa` at the end of import
to tell that this is intentional - no questions asked!    
</p>

#### 4. Create the **routes/** directory

All directories within **routes/** need to have an `__init__.py`.
Again, it goes without saying that there are more ways possible for doing 
this. However, I find this approach comfortable to work with.

Now, we need to organize the routes in our heads based on some fashion.
Structuring routes based on functionality seems a reasonable thing to do,
so let's make it that way.

#### 5. Lastly, create **login.py** inside **routes/**

This is where we write code to handle the incoming request. Again, this
could've been a single file next to our **index.py**, but that would
be inefficient for a larger project.

Finally, the directory structure should look like:
```
LoginFalcon/
|---index.py
|---api/
|     |--- __init__.py
|     |--- auth/
|          |--- login.py
```


### The code

As obvious, we need a resource class. Let's call it `LoginResource`.

The *minumum* steps we need to perform are as follows:
- get the request body from a special variable (usable inside the resource
class)
- add logic to check if supplied credentials are correct (assuming those are
present)
- return an appropriate response that tells the client if credentials are
correct
- document the above behaviour if necessary.

However, there are more possibilities. You can perform pre-processing on the
request body. Say, you need to allow your API to validate the structure of
the incoming JSON, to notify the client about a malformed JSON.
You can do this in two ways - either by using hooks, or middleware. Any way,
we'll come to that later once we're done with the basic code.

The code should look like this:
```py
from api import app
import falcon
import json

# You probably want to get this from a database or something...
TRUE_USERNAME = 'iamuser'
TRUE_PASSWORD = 'secretpassword'


class LoginResource:
    def on_post(self, req, resp):
        username = req.media.get('username')
        password = req.media.get('password')

        if username == TRUE_USERNAME:
            if password == TRUE_PASSWORD:
                resp.status = falcon.HTTP_200
                resp.body = json.dumps({
                    "authenticated": "true"
                })
            else:
                resp.status = falcon.HTTP_400
                resp.body = json.dumps({
                    "authenticated": "false",
                    "msg": "incorrect password"
                })
        else:
            resp.status = falcon.HTTP_400
            resp.body = json.dumps({
                "authenticated": "false",
                "msg": "username not found"
            })

app.add_route('/api/login', LoginResource())
```

We need the client to perform a POST request to out API. Thus, we implement
the `on_post()` method of our `LoginResource` class. This method (all
`on_*()` methods) require at least three arguments - **self, req, resp**. If
you have a dynamic route, it's included as the fourth argument in the method
definition. For example: ```/{username}/posts``` will require `def on_*(self,
req, resp, username):` as the method definition.

Inside the method, we extract the credentials, which are stored in the special
variable `req.media`, using `req.media.get()`.

Now, we proceed to checking whether the supplied values are correct. If they
are, we assign `resp.body` a custom JSON response as shown, to allow the client
know that the supplied pair of credentials is correct.

<p class="note note-dark">
Good APIs have documentation. We need to document what we did in our code,
which includes a description of <i>what</i> does the API return on <i>what</i>
request, on <i>which</i> endpoint.
</p>

We can also assign a status code to our response using Falcon's built-in HTTP
response strings, called **'HTTP_200', 'HTTP_404'**, and so on.

## Serving the API
Now that we have the application ready, we'll need to serve it on our machine.
Unlike Flask, Falcon doesn't come with a built-in server. Hence, we'll use
*gunicorn* to serve our application.

Perform:
```bash
$ pip install gunicorn
$ gunicorn index:app --reload  # from the "LoginFalcon/" dir
```

This serves our API on port 8000 (by default). You can make use of any API
client like Insomnia, Postman or cURL to send a request to
**http://localhost:8000/api/login** with the request body as:
```
{
    "username": "iamuser",
    "password": "secretpassword"
}
```
and obtain a response of `{"authenticated": "true"}` in your response body.
Try changing either of username and/or password fields, and you should get
the appropriate response.

You can use this response in your main application later to know if you
want to log the user in. To add more functionality, you can always add more
endpoints to your application. Also, [Falcon's
documentation](https://falconframework.org) is awesome and definitely worth
going through.

By now, you should have an idea how simple Falcon is, and how closely it
resembles HTTP.
This should get you started with creating your own fast APIs with Falcon.

I intend to write another article about adding middleware and/or
hooks to pre-process requests. Hope this one helped!

<br><br>
Happy Hacking!
<hr>

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
// this.page.url = "https://roshnet.github.io/2019/08/12/unmess-git.html";
// this.page.identifier = "unmess-git";
};
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://roshnet.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
{% endif %}
