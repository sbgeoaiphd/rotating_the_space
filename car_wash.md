---
layout: default
title: Strong Attractors - What the Car Wash Riddle Actually Tells Us About LLMs
---


# Strong Attractors: What the Car Wash Riddle Actually Tells Us About LLMs

There's a thing going around LinkedIn right now. You ask an LLM whether you should walk or drive to take your car to a car wash, and it tells you to walk. The car wash is 100 metres away, so the model pattern-matches to the obvious: short distance, walk. It misses that the car needs to get there too. People screenshot the failure, post it, and conclude that LLMs can't reason.

Nobody is running the experiment properly. I see people comparing across models, which is good, but always at n=1. These are stochastic systems. Every response is sampled from a distribution. Posting a single screenshot is like flipping a coin once and declaring it always lands heads. Without repetitions you can't distinguish between "this always fails," "this fails half the time," "I got unlucky," and "I got lucky and it usually fails." You need to sample to see the shape of what's actually going on.

So I did.

## The experiment

I tested GPT-5.2, the non-reasoning chat model you get when you paste this into ChatGPT, across several prompt conditions. 150 repetitions each, temperature 1.0, with an automated judge classifying each response as DRIVE or WALK.

The baseline, just the raw question with no framing, came in at 59% DRIVE, 41% WALK. More right than wrong, but clearly unstable. Tell the model the question is a riddle: 100% DRIVE. Constrain the answer to a single word, no explanation: 100% DRIVE.

A note on the baseline: an earlier run on a different day at n=10 gave only 10% DRIVE. Whether that's small-sample noise or something else is hard to say. It's not impossible that OpenAI patched a system-level instruction between runs to mitigate a viral failure mode. I have no way of knowing. But either way, the intervention conditions are rock solid, and the public conversation around this is almost certainly shaped by survivorship bias. Someone gets a funny failure, posts it. Someone else tries it, the model says DRIVE, they shrug and scroll on. No post, no engagement.

## What's actually happening

This is a strong attractor. The training data contains an enormous volume of genuine "should I walk or drive" questions where distance really is the relevant variable. The model has learned, reasonably, that short distance usually means walking. That heuristic is correct in the vast majority of cases. The car wash question is the rare exception, and the attractor is strong enough to dominate unless you provide the slightest additional context.

This is an access problem, not a representation problem. The knowledge is in there. The reasoning capacity is in there. The riddle condition proves it. The failure is about default routing: which part of the model's learned space gets activated by the surface features of the prompt. Minimal context breaks it. A framing cue. A format constraint. The attractor is strong but shallow.

This class of shallow attractor is likely to improve with future models, and reasoning models may already handle it better (I didn't test this, but the mechanism makes sense: any model that pauses to reconsider before answering has a chance to catch itself). The car wash may already be getting fixed, which might explain my shifting baseline.

So yes, the failure is real, and yes, it's annoying. But the conclusion most people draw is far too strong.

## The same pattern, much deeper

Here's where it gets more interesting, and more consequential.

I work in Earth observation, using embeddings to find things in satellite imagery at scale. Embeddings are learned vector representations, and there's a rich theoretical tradition in natural language processing about how to analyse and interpret them: linear probes, geometric structure, directional relationships in the learned space. This tradition is well-developed and, for embeddings, largely correct.

When you ask an LLM about working with embeddings in remote sensing, you don't get any of that. You get random forests. Feature importance. Per-dimension analysis. The methodological toolkit of traditional remote sensing, applied uncritically to a representation type it was never designed for.

Drop the words "remote sensing" and just ask about embeddings, and the model assumes you're in NLP territory. Now you get linear probes, cosine similarity, geometric reasoning. The correct toolkit.

The same attractor mechanism, operating at a completely different scale. In the car wash case, "100 metres" activates a distance heuristic. In the remote sensing case, the domain label activates an entire methodological tradition. The training data is full of remote sensing papers using random forests and feature importance, because that's what the field has historically done. The model reproduces the consensus faithfully and confidently, even when that consensus doesn't apply to the specific technical question being asked.

And just like the car wash attractor, you can break it. Say "let's think about this the way NLP treats embeddings" and the model shifts to the correct framework. Or remove the domain label entirely. The knowledge is there. The geometric thinking is there. The domain label is routing to the wrong tradition.

But there's a critical difference in how these two failures play out. If an LLM tells you to walk to the car wash, you'll probably notice. If an LLM tells you your random-forest-on-embeddings methodology looks solid, that sounds right. It sounds like a thorough, methodologically grounded assessment.

From extensive testing, I can say this: if you give the model a paper that conforms to the established paradigm for remote sensing, random forests, feature importance, interpret individual axes, skip linear probes because "they always fail," the model won't push back. It validates and reinforces the approach. It doesn't say "have you considered a geometric perspective?" because the attractor holds. And if you come to it fresh, asking "I'm starting to work with EO embeddings, what should I try first?", you often get routed to that same established paradigm rather than geometrically literate advice. The deep attractor doesn't just fail to correct incomplete methodology. It actively steers people toward it.

## Why this matters

This has implications I find genuinely concerning for how LLMs interact with scientific practice. The risk is less about flattening novel contributions (a paper that already uses geometric methods may well break the attractor by its presence). The risk is about validating methodologically incomplete work, and about steering newcomers toward approaches that miss what embeddings actually offer. Nobody notices the advice they weren't given.

Deep attractors, where the training distribution encodes an entire coherent but misapplied paradigm, won't be fixed the way the car wash problem will. The model would need to recognise that the consensus of a field is misaligned with the technical requirements of a specific problem. That's close to requiring the domain expertise that the reviewer or advisor is supposed to be providing in the first place.

The practical defence is something I've written about elsewhere as *rotations* [LINK]: deliberately reframing what you're discussing with an LLM, shifting the angle, asking "would the answer change if this were in a different domain?" If you ask about remote sensing embeddings and get random forests, then ask "how does NLP handle embeddings?", then ask "is there a principled reason these should be different?", you've mapped the boundary of the attractor. You can feel where the model's default changes, and that tells you something about where to trust it and where to push back.

The car wash riddle is a neat party trick. The deep attractors are where the real work is.
