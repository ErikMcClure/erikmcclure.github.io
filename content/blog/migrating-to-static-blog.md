+++
date = "2018-02-06T09:07:29+00:00"
draft = true
title = "Migrating To A Static Blog"

+++
I've finished constructing a new personal website for myself using [hugo](https://gohugo.io/), and I'm moving my blog over there so I have more control over what gets loaded, and more importantly, so the page doesn't attempt to load Blogger's 5 MB worth of bloated javascript nonsense just to read some text. It also fixes math and code highlighting while reading on mobile. If you reached this post using Blogger, you'll be redirected or will soon be redirected to the corresponding post on my new website. All comments have been preserved from the original posts, but making new comments is currently disabled - I haven't decided if I want to use Disqus or attempt something else. An RSS feed is available on the bottom of the page for tracking new posts that should mimic the Blogger RSS feed, if you were using that. If something doesn't work, [poke me on twitter](https://twitter.com/blackhole0173) and I'll try to fix it.

I implemented share buttons with simple links, without embedding any crazy javascript bullshit. In fact, the only external resource loaded is a Google tracking ID for pageviews. Cloudflare is used to enforce an HTTPS connection over the custom domain even though the website is hosted on Github Pages.

Hopefully, the new font and layout is easier to read than Blogger's tiny text and bullshit theme nonsense.