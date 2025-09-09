+++
title = 'Installing Old Versions with Homebrew'
author = "Sam Craven"
date = 2025-01-25T13:35:00-08:00
#PDT -07:00, PST -08:00
draft = false
slug = "installing-old-versions-with-homebrew"
summary = "Updating Hugo broke my blog! Let's address this issue by using Homebrew to reinstall the old working version."
categories = ["tech", "meta"]
tags = ["tech", "meta"]
featured_image = "/images/brew_rewind.jpg"
+++

To skip the back story click [[HERE]](#installing-an-old-version).

# Why I had this problem

I originally [moved this blog to Hugo]([../posts/moving-to-hugo/) because I was sick of needing to constantly update [Ghost](https://ghost.org/) and those updates breaking my workflow, or worse, my entire blog.

For me, the primary selling point of a static site generator is that it won't have these breaking changes. For years, I can hand it Markdown and it will spit out HTML.

Well, not exactly. Working on another project recently, I updated all my Homebrew packages to make sure I had the latest versions of a few things. It did not occur to me that this would include Hugo. After creating my post, I ran `hugo serve` to preview it and got an error.

```
❯ hugo serve
Watching for changes in /Users/sam/blog.seemsgood.com/{archetypes,content,data,i18n,layouts,static,themes}
Watching for config changes in /Users/sam/blog.seemsgood.com/hugo.toml, /Users/sam/blog.seemsgood.com/themes/ananke/config.yaml
Start building sites … 
hugo v0.142.0+extended+withdeploy darwin/arm64 BuildDate=2025-01-22T12:20:52Z VendorInfo=brew

ERROR deprecated: resources.ToCSS was deprecated in Hugo v0.128.0 and will be removed in Hugo 0.143.0. Use css.Sass instead.
ERROR deprecated: .Site.Social was deprecated in Hugo v0.124.0 and will be removed in Hugo 0.143.0. Implement taxonomy 'social' or use .Site.Params.Social instead.
Built in 1726 ms
Error: error building site: logged 2 error(s)
```

Both of these are errors with [my theme](https://github.com/theNewDynamic/gohugo-theme-ananke/).


As I see it, I have three options to fix this issue.

1. Fix my theme
2. Change themes
3. Revert to a version of Hugo that works with my theme

## Fix my theme

My memory is that this theme hasn't been updated in quite a while[^1]. If that's the case, I'll need to update the code myself. That's a rabbit hole I'm not willing to go down.

[^1]:It turns out that my memory was _almost_ correct. However, the theme's development recently became active again and it even has a new primary maintainer.

## Change themes

I'm specifically trying to avoid breaking anything so totally changing themes seems like a step in the wrong direction.

## Revert Hugo

This seems like the easiest way forward. Hugo is open source and I installed it with Homebrew, so I'd imagine there's got to be a relatively straightforward way to get an older version of Hugo working on my system.

# Installing an old version

## tl;dr

1. Find the Homebrew formula in the Homebrew repository
2. Go through commit history to find an old version that will work for you
3. Download that `.rb`
4. `brew install /path/to/some_formula.rb`

## Full steps

Homebrew does have a built-in way to install old versions (`brew install some_package@some_version`) but not all packages are built to support this installation method. Unfortunatelly, Hugo is one of these.

This method comes from [this Stack Overflow answer](https://stackoverflow.com/a/7787703):

Before doing anything, I'll need to uninstall the current version with `brew remove hugo` .

Next, find where this Homebrew formula exists in the [Homebrew GitHub repo](https://github.com/Homebrew/homebrew-core). For Hugo, that's [https://github.com/Homebrew/homebrew-core/blob/master/Formula/h/hugo.rb](https://github.com/Homebrew/homebrew-core/blob/master/Formula/h/hugo.rb) .

We want an old version of this file so click **History** at the top of that page to get to [a list of commits on this file](https://github.com/Homebrew/homebrew-core/commits/master/Formula/h/hugo.rb).

My error message indicates that I'm going to want a version before 0.124.0 . I'll have to go back a few pages before I find the commit for 0.123.8 . Click **View code at this point** (the middle of the three icons on the right side) to get to [the old version of that file](https://github.com/Homebrew/homebrew-core/blob/891690a5ffe46e531222b10f909889da2bc0cea6/Formula/h/hugo.rb)

Click the download icon to download this old Homebrew forumla.

Install this old version with `brew install /path/to/some_formula.rb`

Finally, and perhaps most importantly, write this blog post so that when you break something again, you don't have to spend a bunch of time researching a solution

## tl;dr

1. Find the Homebrew formula in the Homebrew repository
2. Go through commit history to find an old version that will work for you
3. Download that `.rb`
4. `brew install /path/to/some_formula.rb`

# Where I go from here

I see that this theme has started active development again. I thought I'd set things up in such a way that I'd be getting those updates but apparently not. I suppose I should look into that.

I've also been wanting a new theme for some additional features so it's possible I don't even troubleshoot this problem any further and simply migrate.

I hope this was helpful - thanks for reading!

---

[Beer image](https://commons.wikimedia.org/wiki/File:NCI_Visuals_Food_Beer.jpg) by Len Rizzi (photographer), Public domain, via Wikimedia Commons

[Rewind symbol](https://commons.wikimedia.org/wiki/File:Eo_circle_grey_rewind.svg) by Emoji One, [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) via Wikimedia Commons
