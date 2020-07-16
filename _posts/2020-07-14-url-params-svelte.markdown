---
layout: post
title:  "Accessing URL params in Sapper"
comments: true
date:   2020-07-14 21:31:22 +0530
vue_route_snippet: "{{ $route.params.username }}"
---

Coming from the [vue-router](https://router.vuejs.org) domain, I've never had
trouble accessing the dynamic URL routes in any component.

However, it's a bit different in Sapper.

The docs are nice, but didn't fix the issue I have had.

I learnt this the hard way. It was only after hours of failed attempts that I
was able to figure it out, how exactly URL params work in Sapper. And more
importantly, a big idea about how Svelte is different in this aspect.

In Nuxt.js (or Quasar, or anything that essentially uses vue-router), we can
access the dynamic URL parameters from the component in the following way
(assuming that we have *"/greet/:username"* registered in vue-router's `path` or
`children`):

```vue
<!--
For example, this file is
- "/pages/greet/_username.vue" for Nuxt.js
- "src/pages/greet/_username.vue" for Quasar framework
-->
<template>
  <div>
    <h2>
      Hello, {{ page.vue_route_snippet }}!
      Hope you're doing great.
    </h2>
  </div>
</template>
```

To obtain this functionality in Sapper, we use the `[slug].svelte` naming
style.

To create a route whose dynamic part is to be called `username` in the
component, the file must be named:
  - **`/pages/greet/_username.vue`** in (say) Nuxt.js
  - **`/src/routes/[username].svelte`** in Sapper.

I wish there was an easy way to access the value of `username` in component,
but it's a bit more unintuitive than I expected.

## Accessing the parameter

```html
<!-- This is /src/routes/[username].svelte -->

<script context="module">
  export async function preload ({ params, query }) {
    return { username: params.username }
  }
</script>

<!-- Regular Svelte below. The parameter must be first extracted from the
argument to preload(), and then returned to be made available in a regular
Svelte component.
-->

<script>
  export let username;
</script>

<h3>Hey, { username }!</h3>
<p>Hope you're doing great!</p>
```

Creating the first `<script>` should be done in module context. Not doing so
will result in having two `<script>` tags, which is against writing Svelte
components. Also, wrapping both exports in a single script renders the value
`undefined` without errors.

Based on what I get from the docs, I speculate that its because `preload()`
executes when the module initializes, which is before the component renders.
So, `username` gets populated once the module has rendered. The (later)
initialized component can then access it to display a nice greeting.

{% if page.comments %}
<div id="disqus_thread"></div>
<script>
var disqus_config = function () {
this.page.url = "https://roshnet.github.io/2020/07/14/url-params-svelte.html";
this.page.identifier = "url-params-svelte";
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
