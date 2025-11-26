+++
date = '2023-05-21T10:00:07+05:30'
title = 'My Experiments With Copilot'
+++
I've been trying my hand at creating something with [Copilot](https://github.com/features/copilot) for the last two weeks. The one question that I set out to find an answer is, 

> "Can copilot really replace pair programming?"

{{< figure src="/images/copilot.png" title="Human and Robot pairing. Image created by Midjourney" >}}

I've spent a good amount time of my career crafting code and 80% of the time the code was co-created with my pair. So, there was an initial hesitation to try this out. But one fine day, I crossed the chasm and got started on my experiments with copilot.

## Quick start

The way copilot responded to some of my initial few prompts was amazing. I was writing a simple calculator in three different languages. In a few minutes, I had the code and tests for my calculators in `Java`, `Python` and `Elixir`.

## A (not so) complex problem
I encountered challenges when I prompted copilot to create a simple CRUD API in layered architecture. 

Given the prompt to create an article controller, copilot suggested to do the CRUD for an article and then it stopped there. My initial reaction was, 

> "Dear copilot, I know that I'd need to have CRUD endpoints in a controller. Could you please write the code for the same?"

But then there was only silence from the other end. So, I gave up and started defining the controller and dependencies. At this point, copilot came along and offered some suggestions that were appropriate.

It failed me again though. Given the prompt to create an endpoint to create an Article, copilot went on to define an action in MVC style, whereas I was looking to create a REST API.

## Lessons learnt

1. You need to get better at prompt engineering. Be able to break down the task at hand and give specific prompts to copilot. The more specific your prompt is, the more appropriate the code will be.

2. Copilot can get stuck at times, or produce incorrect code. In such cases, once you take a lead, copilot will be able to follow.

3. You need to really know what you are doing. In a new language / framework, it can easily (and quickly) lead you down the drain.

## Testing time

Once I had the code in place, I went on to explore copilot's skills in writing tests. I must admit that, it was pretty impressive in this space. It was able to cover the happy path and error scenarios equally well.

Again, Copilot's is quick to pick up your coding style and write more tests in the same style.

While at it, I wanted to write a builder for my model and with just one line of prompt, copilot gave me the implementation of the builder class. This is a key improvement. If copilot is able to write code understanding the language of software developers, that would be a good improvement in this tooling.

## Can copilot replace pair programming?

The verdict: `Nope. Or at least, not yet.`

> Copilot doesn't know (or care) why you are doing something. It is highly focussed on the how. While pairing often you'd have to zoom out and look at the big picture to ensure you are on the right track. Copilot is very contextual and can help you with specific tasks, but cannot help you with doing the right thing. It helps you do the thing right.

There is still a long way to go. But the future seems promising. [CopilotX](https://github.com/features/preview/copilot-x) is a good step in that direction.

## Future experiments:

1. Can you drive design through TDD?
Can you just write the tests and let copilot write the code. You should be good as long as the tests are green.

2. Can copilot help you go deeper? 
Once you decide on the technical architecture, can copilot write code to create the entire infrastructure? 
(or)
For a ML problem, can copilot help you pick up, train and test the best model for the job?

