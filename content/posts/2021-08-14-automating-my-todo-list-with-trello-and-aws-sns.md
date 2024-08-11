+++
title = "Automating My Todo with Trello and AWS SNS"
author = "Sam Craven"
categories = ["tech", "python"]
date = 2021-08-14T18:28:04Z
draft = false
featured_image = "/images/trello_sns.png"
slug = "automating-my-todo-with-trello-and-aws-sns"
tags = ["tech", "python"]
+++

A couple years ago, Alice Goldfuss posted [this blog post about how she handles her todo list with GitHub's Kanban feature and Twilio](https://blog.alicegoldfuss.com/automating-my-todo/). I really liked the idea and set out to duplicate it. However, I prefer Trello and since most of my projects are already on AWS, I wanted to use their [Simple Notification Service](https://aws.amazon.com/sns/) to send the text messages.

I have since stopped using this automation. The move to working from home changed my daily workflow in ways that made a Kanban board not as useful a mechanism for me. Some recent changes to how SNS is billed were the final nail in the coffin.

I wanted to publish this code in case it's useful to anyone else. Unfortunately, as it's been over two years since I wrote and deployed this, how to deploy it in Lambda (or elsewhere) is an exercise left to the reader.

The full code may be found [[here]](https://gist.github.com/thecravenone/4a2294d0d5bb5180bca90323e8a8d4cb).

One note - board IDs are _not_ static. If you add or remove boards, the board IDs will change. You should probably be figuring out the board ID each time around. If you're not adding/removing boards, you can set them statically. To pull the board IDs:

```Python
all_boards = trello_client.list_boards()
for i in range(len(all_boards)):
	this_board_name = all_boards[i].name
	print(str(i) + ": " + this_board_name)
	this_board_lists = all_boards[i].list_lists()
	for j in range(len(this_board_lists)):
		this_list_name = this_board_lists[j].name
		print("     " + str(j) + ": " + this_list_name)
```
