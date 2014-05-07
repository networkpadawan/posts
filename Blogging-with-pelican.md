Title: Blogging With Pelican
Slug: blogging-with-pelican
Date: 2013-11-12 13:04:17
Tags: blogging, pelican, python

So I have already told that I'm back to the blogging business(don't know about the business part...but i digress) and have already a few posts lined up to the near future.

But I wanted to post here the stuff I'm using to blog, mainly the engine behind these pages you see.
I was going to use wordpress (as almost everybody, and ain't nothing wrong with that!) but I need to learn to code, devops is the rage, and networking is following that path also, so I decided for a static generator which lets me check under the hood what's going on. I already said that I'm using [pelican](https://github.com/getpelican), and the theme is based on [bootstrap3](http://getbootstrap.com/). 

Pelican is great because it allows me to create scripts/plugins to enhace the page. I already developed a search script based on [whoosh](https://pypi.python.org/pypi/Whoosh/), that gives me full text search, and another script that allows me to tweet the last blog post.
I also use mdx_video to embed youtube/vimeo links and fitvids to make everything "responsive" a.k.a. looks good both on desktop and mobile.

I'll be pushing everything to github and try to better integrate whoosh with pelican.

If you find any bugs, pleace comment or contact me!

(Thanks [@eloisavaldes](https://twitter.com/eloisavaldes))