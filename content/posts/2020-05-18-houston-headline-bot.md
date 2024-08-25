+++
title = "How my /r/Houston Headline Bot Works"
author = "Sam Craven"
date = 2020-05-18T22:32:13Z
draft = false
slug = "how-my-r-houston-headline-bot-works"
summary = "A while back, the mods of /r/Houston implemented a rule prohibiting \"poor headlines\" on post submissions. While what exactly this means isn't defined, I decided to build a bot to call it out when it happens."
categories = ["tech", "python", "reddit"]
tags = ["tech", "python", "reddit"]
featured_image = "/images/rHouston-3.png"
+++



# Preface

A while back, the mods of /r/Houston implemented a rule prohibiting "poor headlines" on post submissions. While what exactly this means isn't defined, I decided to build a bot to call it out when it happens. When a link to a news site is posted with a title other than the headline on the news site, the bot responds with the original title of the article. (Some examples: [[1]](https://www.reddit.com/r/houston/comments/fszlj0/abbott_says_churches_can_open_again_deemed/fm45szi/), [[2]](https://www.reddit.com/r/houston/comments/exd4kr/looks_like_angela_nguyen_has_been_found_great_news/fg7iqop/), [[3]](https://www.reddit.com/r/houston/comments/fxqywe/parents_and_students_who_went_to_hisd_and_picked/fmvvvuk/))

This post is intended both to describe how the bot is built and for me to talk through some of the compromises I've made in the way it works. I'll also note that the code here is a simplified version of what's actually running. The code presented here is 23 lines, while what's running as of this post is 57, consisting of logging, messaging, and moving some things into their own short functions.

As you might guess from that line count, the bot is quite simple. Its action can be broken down into three steps.

1. Get a list of posts to check
2. Check whether a post's title matches its article's headline
3. Respond to posts that violate the rule

Let's get into it.

# Step 0: Connecting to the Reddit API

Easy peasy, just read the docs. This section is only here so that it's clear that I've already done this part.

```Python
import praw
reddit = praw.Reddit(client_id="MY CLIENT ID",
                     client_secret="MY CLIENT SECRET",
                     username="MY USERNAME"
                     password="MY PASSWORD",
                     user_agent="/r/Houston headline bot")
```

# Step 1: Get a list of posts to check

The bot runs on a [cronjob](https://en.wikipedia.org/wiki/Cron) once every five minutes. It fetches the newest submissions to the subreddit and loops over all of those that were submitted in the last five minutes. As soon as it reaches a post older than five minutes, it exits.

```Python
from datetime import datetime
import sys
runtime = int(datetime.now().timestamp())
for submission in reddit.subreddit('houston').new(limit=10):
	subtime = submission.created_utc
	if subtime > runtime - 300:
    	# Proceed to Step 2
    else
    	sys.exit()
```

Well just typing that into the blog, I realize I can save a teensy bit of RAM by only importing the `exit` function from `sys`.

There's two limitations/compromises here.

## Only checking the last five minutes

There's a couple situations in which checking the last five minutes would be insufficient to check every single post.

First, and most likely, is that the cron simply doesn't run in the appropriate window. Right now, it's running on my desktop computer. An ISP outage, power drop, or even just me being booted into another OS are all common ways this could occur. This was originally planned to be run on [AWS Lambda](https://aws.amazon.com/lambda/) which would eliminate all of those issues.

Second is a weird quirk of timing. It is _possible_ that between execution time and waiting on the API, a post could fall into a tiny window. I chose to simply accept this possibility as a rare missed post would be preferable to responding to an offending post twice.

Both of these problems can be solved by externally storing the last checked post and checking all posts since that one. The data could be stored in a database, flat file, or environmental variable. This is a change that I do plan to make eventually. However, I'm weighing how far back the bot should be able to respond. For example, there is little value in responding to a days-old post to point out that its title and article headline do not match.

## Only getting the last ten posts

The API call only gets the last ten posts submitted.

```Python
# [...]
for submission in reddit.subreddit('houston').new(limit=10):
# [...]
```

It is very uncommon for /r/Houston to experience ten or more posts in a single five minute period. That said, the limit was originally put there when I was planning to put this on Lambda, where the bot would be substantially more memory limited than it is running on my system.

# Step 2: Check whether a post's title matches its article's headline

This takes a few more steps. Every website has its own way of displaying a headline. I've created a regular expression for each supported news site. If a post is a link to that site, check if the title matches the appropriate regular expression.

First, check whether a post is a [self post](https://www.reddit.com/r/help/comments/16secs/what_is_a_self_post/) (ie, not a link to an outside website).

```Python
if not submission.is_self:
```

If the post is a link to an external website, scrape its domain out of the URL, as that's what we'll use to determine which regex to use.

```Pythong
url_regex = "(?<=\/\/).*?(?=\/)" # This line is outside of the loop

# Inside the loop:
domain = re.findall(url_regex, submission.url)[0]
```

The regexes are stored in a dictionary with the domain as the key and the regex as the value.

```
regexes = {
	"SomeDomain.com":"SOME REGEX",
    "SomeOtherDomain.com":"SOME OTHER REGEX"
}

# Inside the loop:
if domain in regexes:
	# Check if the post title matches the headline
```

Now for the actual comparison. Get the headline used by the site by retrieving the site's content, [normalize](https://en.wikipedia.org/wiki/Canonical_form#Computing) it (in this case converting [HTML entities](https://www.w3schools.com/html/html_entities.asp)), and comparing it to the regex. Then compare this to the post's title (also normalized by trimming trailing white space that commonly happens from copy+paste issues).

```
site_content = requests.get(url).text
headline = html.unescape(re.findall(regexes[domain], site_content)[0].rstrip())
if headline != submission.title.rstrip():
	# Headline doesn't match post title. Comment on the post
```

## Issues

There's a handful of issues here.

### What's a headline anyway?

What, exactly, constitutes the headline of an article? There are three obvious places to look for it, and these sometimes differ.

* The big text at the top of the article
* The `og:title` tag in the [Open Graph](https://ogp.me/) data (I'm pretty sure this is what Reddit uses to suggest headlines
* The `<title>` tag of the website, often expressed as "News Source Name: Headline"

I think that _ideally_, news sources wouldn't be providing different headlines in different places. This is especially troublesome as the Open Graph data is intended for previews on social media, and therefore may be more clickbaity than what's actually displayed on the page.

Even if what's displayed on the page is different than the Open Graph data, I don't want to call people out for using Reddit's recommended title.

To again use the word "ideally," _ideally_ I would be checking every place that a headline could be found and considering a title that matches any of those to be correct.

### Headlines change

It's not uncommon to see a news source change their headline as an article is updated with more information. I've decided to accept that risk as it's unlikely that an article's title would be changed in the at-most-five-minutes gap between when it is posted to Reddit and when the bot runs.

### Parsing HTML with regular expressions

[Parsing HTML with regular expressions angers the gods](https://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags/1732454#1732454).

It may be possible to make things lighter weight with a library that reads Open Graph data but this comes with its own problems.

* I'm chasing minor efficiency boosts when I could be improving functionality
* Until my regex list gets to be quite long, any library is probably heavier weight anyway
* Requires the site be using Open Graph data

I'm primarily focused on that last one. For all these reasons, the bot will continue to work on regex'd HTML for the forseeable future.

### Website formatting changes

In fact, it sometimes changes so often that I gave up on my previous project of [scraping the AP poll](__GHOST_URL__/using-grep-to-scrape-the-ap-poll/) as it changed several times in a single season.

Short of using something like an Open Graph scraper, I don't think there's a way to work around this. Fortunately, the in-use version of the bot messages me every time it comments on a thread, so I'm usually aware of a regex failure within hours at most. If I'm at home but don't have the time to fix the failure, I'll simply comment out that domain's regex line until I can get to it. In the worst case scenario where I don't have access to my home system, I've simply changed the bot's password so that the bot couldn't post. That issue is another that is avoided by using something hosted remotely.

### A site could have more than one headline format

This has been the most common cause of failures. A site has multiple formats depending on how the information is presented. For example, the format might be different between the mobile, video, and breaking news sections of the site.

In a few cases, I've been able to refine the regex in a way that worked on multiple formats. In one other case, I added a check specifically for a certain section of the site and changed what regex to use. This isn't great for a one-off but it's night-unmaintainable to continue doing. I'm not sure how I'll go about correcting this in the future. Is replacing the value for each domain with an array of possible regexes the way to go? Feedback on this aspect is definitely appreciated.

Anyway, now that the bot's done all its checking, it's time to move on to.

# Respond to posts that violate the rule

This is again simple Reddit API stuff.

```Python
# If the bot should respond:
reddit.submission(id=submission).reply('Original headline:\n>#' + headline)
```

# Putting it all together.

Because the code is pretty spread out above, here it is altogether,

```
from datetime import datetime
import praw, re, requests, html, sys, creds

reddit = praw.Reddit(client_id=creds.client_id,
                     client_secret=creds.client_secret,
                     password=creds.password,
                     user_agent='/r/Houston headline bot',
                     username=creds.username)

regexes = {
	"SomeDomain.com":"SOME REGEX",
    "SomeOtherDomain.com":"SOME OTHER REGEX"
}
url_regex = "(?<=\/\/).*?(?=\/)"

runtime = int(datetime.now().timestamp())

for submission in reddit.subreddit('houston').new(limit=10):
	subtime = submission.created_utc
	if subtime > runtime - 300:
		if not submission.is_self:
			domain = re.findall(url_regex, submission.url)[0]
			if domain in regexes:
				site_content = requests.get(url).text
				headline = html.unescape(re.findall(regexes[domain], site_content)[0].rstrip())
				if headline != submission.title.rstrip():
					reddit.submission(id=submission).reply('Original headline:\n>#' + headline)
	else:				
		sys.exit()

```



