+++
date = '2023-02-19T06:41:13+05:30'
title = 'Demystifying XP Values, Principles & Practices'
+++

For any team that wants to adopt XP, it is important to make sense of the [values](http://www.extremeprogramming.org/values.html), principles and practices and how they are connected to each other. Though, this understanding will solidify itself over time, it can be challenging to the team to sustain till then. And many teams may not have the luxury of allowing themselves the required time. Here, I attempt to put my understanding from practising XP over my years in Thoughtworks into words, hoping it'd help someone who is looking for this.

## An Analogy:
Imagine a child, that is learning to speak. Children usually start speaking and create grammatically correct sentences even before they understand the elements of grammar. At this point, they are simply replicating / mimicking what they hear others speak. But, as they grow, they start understanding what they speak. They are able to break it down and label things as nouns / verbs / adjectives / adverbs. They are able to understand the different voices in the sentence. Once they understand this, they'll play around with it. They'll be able to generate more sentences that mean the same. They become capable of expressing themselves creatively and in different forms and without taking much of cognitive load.

## In the XP world:
I feel, it is the same with teams that want to practice XP. Often it is useful to start with the practices, since they are more concrete. For example, you can start doing TDD even before you could understand the values of doing so. You can write a test - see it fail (bleeding red) - write the code - and see it pass. These are tangible things. The practices are activities that helps us in our day jobs. We spend most of our time in such practices. It is a very good place to start. But, if we don't understand the values and principles behind them, we risk being dogmatic.

As the team matures, we need to look beyond practices and be able to look at a big picture, be able to pick & choose (or drop) what is appropriate for the given situation.

Consider a scenario to see how these different components come to play with each other. As a team, we've agreed to value `Communication`. Personally, I'm not a big fan of code commenting. However, there are people on my team who'd see code comments as documentation that accounts to communication. I'd suggest that we address this by adding tests, which is guided by the principle of `Quality`.

However, if the team publishes an API and if anyone in the world can consume it, then it becomes essential that we add documentation (which could be in the form of comments and using tools to generate docs out of it). Here the documentation is important since it is now guided by a couple of principles `Humanity` (for external devs who consume our APIs) and `Redundancy` (by having tests + docs).

{{< figure src="/images/xp-values-practices-link.jpg" title="Mapping to show adoption of different practices based on context" >}}

## The Golden circle of XP:

I relate the values, principles and practices to [the golden circle](https://www.ted.com/talks/simon_sinek_how_great_leaders_inspire_action). The practices (what we do) form the outer most circle. The principles (how we do) takes the inner circle. The values (why we do) goes into the core.

Lets say, As a team we decide to do TDD. Practising TDD would be hard, if we take a complex problem and attack it as a whole. We'd have to break it down to smaller pieces and attack one at a time - which essentially means we are taking `Baby steps` - which in turn reflects the fact that we value `Simplicity`.

{{< figure src="/images/xp-golden-circle-tdd.jpg" title="Golden circle showing the route from TDD to Simplicity by taking Baby Steps" >}}

Similarly, if we take another example - when the team decides to do pair programming. We could trace it to the value of Feedback in the lens of Redundancy.

{{< figure src="/images/xp-golden-circle-pairing.jpg" title="Golden circle showing the route from Pairing to Feedback by allowing Redundancy" >}}
