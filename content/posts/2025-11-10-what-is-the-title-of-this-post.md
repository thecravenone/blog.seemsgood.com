+++
title = 'What is the title of this post? (And why is that question so hard to answer?)'
author = "Sam Craven"
date = 2025-11-10T18:50:00-07:00
#PDT -07:00, PST -08:00
draft = false
slug = "what-is-the-title-of-this-post"
summary = "A brief look into the different ways websites express titles and why that's confusing and a little bit a problem."
categories = ["tech"]
tags = ["tech", "reddit"]
featured_image = "/images/newspaper-headline.jpg"
+++

# What's in a title?

When you post a link somewhere, most platforms want to show a preview of that link. While it would be great to use that big text at the top of this page (hereafter referred to as its "headline"), identifying that text across every different website on the internet would be impossible. The solution to this problem is obvious: use some sort of metadata.

HTML already allows for a page to express its title via the [title element](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/title) ("title tag"). That's what appears at the top of your browser window or tab. In the early days, that's what most sites would use for their link preview (assuming they generated link previews at all).

Using a page's title tag often resulted in a less-than-desirable results. The solution to this problem is another metadata tag.

Skipping ahead a bit, [Facebook's Open Graph protocol](https://ogp.me/) (og) has become the de facto standard and is widely used by non-Facebook entities, including Reddit, LinkedIn, and Discord. (Twitter/X will use og tags but prefers its own tags.) Open Graph expands beyond a title and allows a website to specify what type of content is at the link, a summary, a preview image, and more.

# Why does this matter?

A page's headline and title tag are easily visible to the user. Open Graph and other similar metadata is not.

It's not unusual for metadata to be invisible to the user. But when a user posts one of these links, this metadata appears on their post, probably the first time they've seen it. Where this gets particularly interesting to me is moderation.

## Moderating titles

I think there's a few places where this "hidden titles" possibility could create moderation problems but I'm most specifically interested in Reddit.

Many subreddits require that users post articles only with their original titles. Reddit (the platform) has a built-in function to suggest a title which uses the og title field. From a user perspective, Reddit may be suggesting a title which a subreddit's rules indicate is not allowed. From a moderator perspective, a post may be submitted with a title that it appears there own rules do not allow.

To be clear, I do not believe this is a problem with the protocol. This is a problem with the websites that specify multiple different titles for their content depending on how you look for the title. In my experience when the og title differs from the headline, it is usually a more clickbaity title. I think this is especially ironic because [Open Graph's Design Insights deck](https://www.scribd.com/doc/30715288/The-Open-Graph-Protocol-Design-Decisions) specifically mentions that SEO has caused the title tag to become less useful. Now the og title tag is used in the same way, optimizing for clicks on social media.

Moderating titles in this manner is further complicated by sites, especially news sites, changing titles and headlines after they've been posted. One particularly egreious site I found continued to use the same URL for every update to a particular subject for weeks, changing only the headline. I would not be at all surprised to hear that news orgs are A/B testing titles or headlines. It would even be trivial to display different titles or headlines based on who's requesting it, where they are, or various other aspects of a particular user (or platform!)

---

Feature image by Rufus330Ci, Public domain [via Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Land_on_the_Moon_7_21_1969-repair.jpg)
