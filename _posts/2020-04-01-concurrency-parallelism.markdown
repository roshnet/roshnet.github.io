---
layout: post
title:  "Concurrency and Parallelism"
comments: true
date:   2020-04-01 12:21:56 +0530
---
I had a hard time understanding those terms, until I implemented them in code.

What’s common in these two, is the high level effect they have on an
application’s overall performance. If it were not for these things, I believe
the user experience would be a mess.

Perhaps, there would be no such thing as a "New Tab" in browsers, nor could
we open multiple apps, or our entire devices would need a reboot if one
application got stuck in a task. Woah, that'd be hell.

## Concurrency
Imagine you’re making tea. You boil the water. That takes a while. You don’t
wait for the water to boil completely before adding the other stuff to it.

That’s concurrency at it's core - doing other tasks while a slow operation
proceeds. Please don't assume "slow" to be literally slow - it implies a
time-taking process.

A feature of interest here, is that only a single person does all the tasks.
The net efficiency of the process depends on the person who's making it.

The "person" happens to be a core of the CPU. Yes, a single core CPU can
execute tasks concurrently. It's the "switching" that enables multi-tasking -
"do 'B' while 'A' completes, and then switch back to 'A' when it's done".

## Parallelism
10 workers take 30 days to build something => 20 workers will make it 15.

This is parallelism. The work mostly happens independent of any other workers,
but in a coordinated manner.

Of course, this means that parallel processing will actually utilize more than
one of the available CPU cores for the same job.


### Is one "better" than the other?

It might be tempting to think that parallelism is the better option out of the
two, but it really depends on the problem being solved.

For instance, consider a search algorithm. There’s a long, long array,
which needs to be searched for an item. A naive solution will be to apply a
simple for-if construct. With parallelism however, a possible solution is to
start two threads from the two extremes ends of the list. The desired result
could be obtained in nearly half the time (averaged).
Note that using concurrency here won't be a good idea, because at the end, the
latter thread will start only after the former has finished. It would be as
good as the approach with a simple for-if.

Another example can be a web client which consumes a resource (say, an API)
over the network, to display some content to the user. Here, concurrency would
be the preferred choice, as you don't want your app to get stuck until the
data is downloaded (JavaScript and Golang are good to work with those kind of
tasks). Instead, a call to the API can be triggered, and the other processing
can be done while the content downloads. The asynchronous operation being
carried out in this case is a common example of concurrency.

Also, in a way, using parallel processing might even jeopardize the CPU core
being used for the network operation, as the server that hosts the API may
take long to return the response. It's expensive to dedicate an entire core to
a process that can wait a few milliseconds.

#### Footnotes
If anything in this post seems off, please point them out in the comments.

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "https://roshnet.github.io/2020/04/01/concurrency-parallelism.html";
this.page.identifier = "concurrency-parallelism";
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
