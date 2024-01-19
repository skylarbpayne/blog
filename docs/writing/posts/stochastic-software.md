---
draft: False
date: 2024-01-19
slug: probabilisticprobabilisticprobabilisticprobabilistic-software
categories:
  - Personal
authors:
  - jxnl
---

# A paradigm shift for probabilistic software

This writing stems from my experience advising a few smaller startups. In particular, ones where there are plenty of junior software engineers trying to make a transition into machine learning and related fields. From this work I've noticed three things that I want to call out, address, and my goal is by the end of this article these younger developers will have a few questions they can ask themselves in order to have a better bias for decision making under uncertainty.

1. Would I make more progress if I set up an experiment?
2. Do I know what I want to measure and improve when I set up this experiment?
3. What is the decision that I'm going to make as a result of seeing the results of this experiment?
4. What are the conditions under which I will revisit this if I see neutral or negative results?
5. Can I update my mental model of the system based on the results of this experiment, to plan future work?

<!-- more -->

## Who am I?

I really want to highlight the difference between software and science. In college I studied physics and computational mathematics and I've never considered myself a software engineer. I've done research studying computational social science, and epidemiology. I've worked at Facebook to build models that can detect and priotitize content moderation work flows, and I've built probabilistic systems at Stitchfix with vision models, product search, embeddings, and recommendation systems.

None of these are things that I would consider software in the classical sense. None of these things would have been features that could have been built on some kind of sprint. Instead, these are all probabilistic systems that require a lot of exploration and experimentation to build.

Now I am a technical advisor and I work on software teams to help them level up their machine learning capabilities but also to coach and mentor junior engineers to think more probabilistically.

I noticed a few of the pitfalls that folks are running into these days and I want to call them out.

## What is Probabilistic software?

I mostly call it probabilistic software, but what I'm really talking about is systems that use machine learning. Where we look at probabilities and distributions of things rather than discrete interactions, like an API call from one server to another.

Instead, we are ranking things. We are sorting things. We are grouping things in a fuzzy manner in order to build features like recommendation systems, retrieval applications, or even the hot thing these days, which are agents powered by LLMs.

## Edge cases and long tails

> you can think your way into solving a deterministic system, but you cannot think your way into solving a probabilistic system.

The first thing that I want to call out is that deterministic software has edge cases while probabilistic software has long tails.
I'm finding a lot of junior folks try to really think hard about edge cases around probabilistic systems and truthfully it doesn't really make sense. It is unlikely that we can enumerate and count issues ahead of time, we can only work in percentages and probabilities.

Instead, we should be focusing our efforts on segmenting and clustering the distribution of inputs and consider solving these problems locally first, before coming up with a hypothesis on how the global system might work.

On top of that, because of these long tails, before deliberating with your whole team on what to do next, I would really suggest asking yourself if we set up an experiment and measure improvements to some metric, do we actually know what we want to measure?

We should question the tolerance that we have for these systems. And what are acceptable thresholds for something like precision and recall, rather than asking ourselves if it will work or not work?

## Designing experiments and metrics

> All metrics are wrong, some are useful.

All this deliberation on the edge cases and the long tail stems from the fact that we are not actually thinking hard enough about what the experiment should be and what the metrics should look like.

The goal of building out these probabilistic software systems is not a milestone or a feature. Instead, what we're looking for are outcomes, measurements, and metrics that we can use to make decisions. We are not looking for some notion of test coverage. Instead, we're looking at the trade-offs between precision and recall, whether accuracy is a good metric for an imbalanced dataset, or whether we can improve our evaluations effectively under some other constraints.

Well, it is obviously important to deliberate over database schemas and API contracts early in the process. When we're building probabilistic systems like a recommendation system or a RAG application, it's very important to also focus on what kind of outcomes we're trying to drive. Even if we don't have some business outcome
(like churn or conversion), it's still valuable to have local, smaller, short-term outcomes like model accuracy or some LLM Evaluation and know that our goal is to prepare a suite of experiments in order to move and change this metric. Does model performance correlate with business outcomes? Maybe, maybe not. But at least we have a metric that we can use to drive decision-making.

!!! tip "Try to focus on what the experiment is going to be and which metric we're going to move and why those metrics are important in the first place. We want to improve AUC because it leads to conversion. We want to improve precision because it leads to a better user experience, and churn, etc."

## Making decisions means to cut off

Now we need to understand that making decisions through outcomes is not actually increasing the scope. We should also get into a habit of using these metrics to drive decision-making that kill other possibilities.

!!! note "Entomology of the word decision"

    The definition of “Decision” actually has Latin roots. The meaning of the word “decide” comes from the Latin word, decidere, which is a combination of two words: de = 'OFF' + caedere = 'CUT'

Once you get in the habit of planning experiments that move metrics, the next skill you need to develop is to actually recommend decisions and actions as a result of these metrics.

Consider this example: We are a data scientist Looking at the different kinds of queries that are coming in for our retrieval application. We've classified a bunch of queries using some classification model and we're doing some aggregates to figure out the count volume and quality of some of these query types.

| Query                | Count | Quality |
| -------------------- | ----- | ------- |
| Personal Data        | 420   | 70%     |
| Scheduling Questions | 90    | 83%     |
| Internet Searchs     | 20    | 9%      |

We noticed that internet searches, for example, are exceptionally low quality and most of our queries are around personal data. The equality metric might be determined by some kind of thumbs up thumbs down rating. We can see that scheduling questions are actually pretty high quality, but there's not a lot of them. From this data you might recommend that we should spend some more time improving the quality of the personal data and we might also decide that due to the low volume and internet searches we might just tell the user that we're not going to service them until we have a better plan.

Here are some recommendations we could be making.

1. We are clearly underperforming internet searches. However, the count is really low.
2. In the meanwhile, we should turn off this feature and know that it won't impact our users too much.
3. Personal data is super high volume and the quality is lacking.

Someone who is familiar with the art of Exploring the data and designing probabilistic software might actually go into the personal data do some more targeted exploration to identify what exactly is doing poorly.

If we have good evaluation metrics then hopefully as we change our prompts or our ranking system or our score thresholds we can rerun these experiments and verify whether something like quality can improve. Since we might not actually be able to measure outcomes from our users, we might want to use third-party evaluation frameworks, but verify first that these actually correlate with the metrics like quality.

## Negative results are still results

It's also important to call out the fact that we're not in academia. A negative result is still a result. The goal isn't to publish novel research, the goal is to figure out how to prioritize. Remember that to make a decision is to cut off. If we get a negative result or a neutral result, then the outcome is the same. We have made a decision. We have made a decision to cut off this line of inquiry, Maybe not forever, but at least for now.

It's also important to trust your judgment too. Just because we're going to cut off this line of reasoning now, it's still good to write up a little memo, explain what happened, write down other things we may not have considered, and answer the question, "Under what conditions will we revisit this line of inquiry?"

## More art than science

I definitely believe that many people transitioning from software engineering to machine learning are often surprised by the empirical nature of the results we obtain. Instead of always having unit tests, we are sampling from the distribution of potential inputs and building a internal model how this system operators. It's much more an art than a science to building these things.

I hope that this article has helped you understand that we should be focusing on outcomes and metrics and experiments rather than trying to think our way through the edge cases and the long tails. I hope that you can also get into the habit of making decisions and cutting off other possibilities. And finally, I hope that you can get into the habit of writing up your results and sharing them with your team so that you can all learn together.

So as you're building these probabilistic systems ask youself:

1. Would I make more progress if I set up an experiment?
2. Do I know what I want to measure and improve when I set up this experiment?
3. What is the decision that I'm going to make as a result of seeing the results of this experiment?
4. What are the conditions under which I will revisit this if I see neutral or negative results?
5. Can I update my mental model of the system based on the results of this experiment, to plan future work?

I hope that this article has been helpful. If you have any questions, feel free to reach out to me on [Twitter](https://twitter.com/jxnlco) or [LinkedIn](https://www.linkedin.com/in/jxnl/).