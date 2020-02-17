---
layout: post
title:  "Lessons from a hackathon"
comments: true
description: "how to think in a hackathon, strategy to follow at hackathons,
become better at hackathons"
date:   2020-02-14 01:47:48 +0530
---
<style>
p, li {color: #164669c2; font-size: 110%;}
h1, h2, h3, h4 {color: grey;}
blockquote {font-family: monospace; font-size: 15px;}
</style>

Recently, I had a chance to attend a hackathon.
We failed, and this post aims at finding possible points of failure.

For a background, we were a team of three, each equally good in the required
tech. Our aim was to build a web application to graphically provide insights
to a student's performace in various areas of interest. The practical aim,
however, targeted school students, thus measuring performance subject-wise.

We didn't have the slightest idea about our app - what shape will it take,
how would the UI look, what architecture should we use, what the hell
did we expect it to do, and so on.

A lot of options were available for an seemingly easy task, but we didn't know
where to start. It was like having a lot of gunpowder, but not enough metal to
create a bullet.

Hours passed, and we landed upon something - a file. Yes, a blank file. Not
exactly a great start, but a start regardless.

Not to mention that we (or I, maybe) still weren't having a sketch of the final
product in mind. Still, we wrote pieces of code which are rudimentary in most
web apps.

While building the app was a priority, learning new things was also an
important one. We asked, "is it really worth it if we used what we already
knew, to create something that can be used to practise something we don't?".
It was fascinating to try building the app using new tools, but the constraint
posed by the deadline was huge.

I've heard people attend hackathons and build their favourite thing using
something they never tried. It's fascinating and thrilling at the same time,
but advisable only when you can accept the possibility of ending up with an
incomplete product if you fail.

We, therefore stuck to our good-old-friendly stack. I now realize that it was
a small mistake.

The next couple of hours were spent coding the different parts of the app (the
UI, the server, and a charting library). Now, this got us somewhere. We, at
the core, had a better understanding of what will the app do. This was
followed by a code base refactor (adding new functionality became hard).
However, at the end, we had "something" to show the judges, which we were
sure wasn't going to cut it.

Be it either way, a lot of things were learnt in the process.


## Lessons, lessons, and lessons...

Once you figure out "what" the app does, it becomes easy to figure out "how"
will you make that happen.

For someone who views your application, all that exists is the front end.
This perhaps implies that it's advisable to start with the front end, and
subsequently add a suitable back end architecture.

I can't emphasize more on how important "knowing your product" is.
Even if you can get a good picture _only_ about how the UI is gonna look, you
are going to be 46x more efficient at building it, compared to without knowing
anything about it. Also, I once read somewhere - the best thing you can do while
choosing a database for your next application is "knowing your data".
So, I'm assuming that the thing which applies to databases, does apply here
too.

I was over-conscious about the architecture and code quality, and thus ended
up using the worst architecture and the least manageable code. I was highly
biased towards creating a general schema for the database. Turns out I
ended up with an app with a fixed schema.

To ensure maximum chances of a success, we needed a leader.
Had a fourth member been as well in the team with some skills, the show was a
go. Some of those skills would've been:
  - ability to "look at the bigger picture"
  - knowing what's possible, and what isn't
  - knowing "who" can do "what".

Of course, there can be additions to this little list.

To me, the idea of a leader does not imply someone who is more skilled
than the rest of the team, but the one who is stable and well-versed enough to
plan a path and motivate (or persuade, if needed) everyone else to follow it.



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
