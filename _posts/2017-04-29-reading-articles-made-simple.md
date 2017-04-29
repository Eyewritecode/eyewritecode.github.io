---
layout: post
title: "Reading Igihe.com articles with Ruby"
date: 2017-04-29
tags: [Ruby]
author: Thibault
permalink: web-scraping-with-ruby
comments: true
---

>>I usually don't read news articles but when i do, it's for the comments.

It all began with a hashtag on twitter [#rwandesetwitter](https://twitter.com/search?vertical=default&q=%23rwandesetwitter&src=tyah) where twitter users(mostly girls) were sharing pictures with that hashtag.
Things escalated so fast as so many people started getting on the hashtag.

it was just a matter of hours until [igihe](http://igihe.com) wrote an article about this.

Now considering how Igihe write their news articles, one can really get what the whole story is about from the title of the article.
When Igihe published the [article](http://www.igihe.com/imyidagaduro/article/hashtag-rwandesetwitter-yaciye-ibintu-kubera-ubwiza-bw-abanyarwandakazi-amafoto) a lot of people reacted to it with different views about the content of the hashtag and the fact that igihe wrote an article about it.

Comments weren't only on igihe really; my twitter feed, whatsapp groups, everyone was talking about the comments as some were really funny.

I didn't have enough to go through all the comments as some people had taken screenshots of it and didn't really feel motivated to read those.

So i decided to let **Nokogiri** do that for me.

I thought of making a ruby script that would scrape articles on Igihe and just show me the article titles and i would choose which one to read its comments.
Things were going to be smooth i thought.
After all, what could be hard about it?
 
- Just visit the url 
- get the links of articles
- get comments

I later realized there were using an iframe to load comments which made me repeat almost the same thing described above.

In the end, i had something working but it could only get me the comments on the first page. I was satisfied and didn't bother continuing working on it.

If you have time to play with it, you can get the code [here](http://github.com/eyewritecode/igihe)
i also made a gif to show how it works.

![xxx]({{ site.url }}/images/igihe.gif)
