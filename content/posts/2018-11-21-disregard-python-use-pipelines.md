+++
title = 'Disregard Python; Use Pipelines!'
author = "Sam Craven"
date = 2018-11-21T03:01:00Z
draft = false
slug = "disregard-python-use-pipelines"
summary = "Your terminal gives you access to tons of small, powerful tools. These can easily be chained together, allowing you to write short programs very quickly. Use them!"
categories = ["tech"]
tags = ["one liner"]
featured_image = "/images/NWO-Pumpe3.JPG"
+++

# Preface

A couple of weeks ago, Tom Scott released this video discussing seven-segment displays and demonstrating some basics of programming.

{{< youtube zp4BMR88260>}}

For those who don't want to watch: Tom introduces the [seven-segment display](https://en.wikipedia.org/wiki/Seven-segment_display), then asks what the longest word is that can be displayed using these displays. He first decides to exclude the letters g, i, k, m, o, q, v, w, x, and z. After some consideration, he also excludes i and o. He finds his answer and then poses a question: What if there's more than one right answer?

I thought this would be a cool thing to blog about and I set to work. First, I replicated Tom's code in Python, because I don't really speak JavaScript. Once complete, I set myself to the task of solving the problem Tom posed at the end of the video. After some brief thought about how I might go about solving this problem, it occured to me that in my normal life, writing a program isn't how I would solve this problem at all.

This sent me down numerous mental rabbit holes about whether I should still blog about this and how to go about it. And that's how I disappointed myself by not having blogged about it the same day Tom's video released.

Nevertheless...

# In Python

First, I replicated  Tom's code. I'm using Python here and I've switched from `continue` to nesting the if statements. I suspect Tom's use of `continue` is for it to be easier for non-programmers to read.

```Python
import re

words_file = open("words_alpha.txt", "r")
words = words_file.read().split("\n")

bad_letters = '[gikmoqvwxz]'
longest_acceptable_word = ""

for word in words:
	if len(word) > len(longest_acceptable_word):
		if re.search(bad_letters, word) == None:
			longest_acceptable_word = word

print(longest_acceptable_word)
```

And of course, let's make sure that works.

```
$ python longest_seven_segment_word.py
dichlorodiphenyltrichloroethane
$
```

Adding `i` and `o` to the regex and again checking that I'm still following along properly:

```
$ python longest_seven_segment_word.py
supertranscendentness
$
```

# Reframing the question

Now for the challenge Tom presented:

>What happens if there was another acceptable word of the same length? There _could_ be multiple correct answers. But I'll leave checking that up to you.

As I began to think about how I would go about doing this, I thought that my usual solution to the initial problem doesn't require any changes to solve Tom's challenge. So here's my question. Is the goal to obtain the answer to the question or is the goal to have a single program that neatly outputs the answer and only the answer?

If the goal is to obtain the answer to the question, I'm not going to code something. I'm going to string together a couple commands using pipelines and have the whole thing done in under a minute.

# The Unix Pipeline
I am greatly unqualified to give a full technical explination of how pipes work so [I'll point you to Wikipedia instead](https://en.wikipedia.org/wiki/Pipeline_\(Unix\)). In short, a pipe takes the output of one command and feeds it into the input of another command. With the power of the tools that come on a standard Linux distribution, an admin can accomplish things far faster than coding them would ever allow.

I'll skip to it. Here's how I found the answer to Tom's question, while not needing to make any changes to also solve his followup challenge.

What was 10 lines of Python has been reduced to 75 characters, 4 of which are spaces that are there for ease of reading and 15 of which are the filename of the dictionary I'm testing against.

Command line:
```
$ egrep -v '[gikmoqvwxz]' words_alpha.txt | awk {'print length, $0}' | sort -n
supertranscendentness
[...]
19 untranscendentally
19 untranslatableness
20 supersuperabundance
20 supertranscendently
20 unapprehendableness
21 superrespectableness
21 supersuperabundantly
22 supertranscendentness
$
```

This oneliner:

* Uses `egrep` to remove all all words containing the letters that can't be produced on a seven-segment display and outputs it to...
* `awk`, which takes that input and prints the length of each word followed by the word itself and outputs it to...
* `sort`, which takes that input and sorts it numerically and ascending.

The last word listed will be the longest word. If there are other words tied, they will be listed above with their length.

# The point is

Your terminal gives you access to tons of small, powerful tools. These can easily be chained together, allowing you to write short programs very quickly. Use them! Truthfully, it took me less time to come up with and write the one-liner than it did to type out the Python code above.

---

Feature image by [Wilfra](https://commons.wikimedia.org/wiki/User:Wilfra) - Public Domain - [Link](https://commons.wikimedia.org/wiki/Pipeline_transport#/media/File:NWO-Pumpe3.JPG)

