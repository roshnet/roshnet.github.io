---
layout: post
title:  "How Git changed the way I code"
comments: true
description: "messed up git, mess git, unmess git, clean git, solutions git"
date:   2019-07-23 17:34:47 +0530
# categories: jekyll update
---

Back then, when I used to work in PHP, I deployed my sites on [000webhost](http://000webhost.com).
The idea of anything called "version control" meant nothing to me.
A friend also once said, "Bro, you should consider using any source code management tool. It's thrilling
to see you change production code inline."
I said, "source code what?".

Later, when my codebase grew; database calls, helper functions and whatnot; I realised why my friend was so
thrilled.

I mastered the typical noob's way of doing version control, how? Simple. Copy and rename the "project" folder
to "project-1", and do shit in the main "project" folder. Hmm, smart, right? LOL.

This was the way until I came across seniors in my college.

Seniors then introduced me to what my friend had been shouting out loud till now - Git.

I came to know how it stores differences between commits, maintains a clean history (if you don't
deliberately mess that up) of versions, and other seemingly magical things it did.

Next thing you know is there's a ".git" folder in all my projects.


{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "https://roshnet.github.io/2019/07/23/git-changed-way-i-code.html";
this.page.identifier = "git-changed-way-i-code";
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