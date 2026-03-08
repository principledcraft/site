---
title: Programming is changing, yet some things stay the same.
date: 2026-03-01
summary: AI tools are changing how we write software, but the principles that make code maintainable haven't budged. If anything, they matter more now.
authors:
  - name: PrincipledCraft
draft: false
---

{{< summary >}}

I'm still working out how to best work with AI tools. Most takes online split into *it changes everything* or *it's a parlour trick*, and I don't buy either one. My productivity has noticeably gone up. Stuff that used to eat an afternoon, scaffolding, boilerplate, first passes at tests, now takes maybe an hour. I've used it for content on this site too.

It's also happy to be wrong. I've accepted suggestions that compiled, ran fine, and still introduced problems I wouldn't have written myself. It sounds sure of itself regardless of whether it's right, which makes it easy to stop checking.

What made the difference was applying the same principled mindset that this site is all about. Not just *write me a test*, but *here's what a good test looks like, here are examples, here's the structure I expect*. Instead of plausible-looking output I had to rewrite, I started getting output I could actually use. After that, a rigorous review and back and forth, always followed by some manual edits.

I keep coming back to the same thought: the craft is changing, but the [principles](/principles) aren't. Knowing what good code and tests look like, caring about simplicity and clarity; those things matter more when you have a tool that generates a lot of code quickly. AI doesn't have judgment of its own. It borrows yours.

[Dave Farley](https://www.davefarley.net/) looked at this more rigorously in his [study of 150 developers using AI](https://www.youtube.com/watch?v=b9EbCb5A408). The finding that stuck with me: more experienced developers write more maintainable code with AI than those with less experience. AI amplifies whatever you're already doing. He also points out that when code is cheap to produce, being principled about engineering matters more than ever, since it's easy to produce a lot of unnecessary sloppy code.

[Jason Gorman](https://codemanship.wordpress.com/2026/01/05/the-ai-ready-software-developer-conclusion-same-game-different-dice/) puts it well: *same game, different dice*. Coding was never the bottleneck in software development. The hard parts, testing, integration, understanding what to build, are still the hard parts. Teams that do well with AI tools are the ones that already had good practices in place: small batches, continuous testing, modular design. The tools changed, but the game didn't.

Over time I've become more deliberate about how I work with it. I give it clear instructions and examples, and I tell it what's out of bounds. I've found it helps to actually sit down and design the workflow together, write down the patterns you want and the standards that matter. I even ask it to flag things that might be worth turning into a permanent instruction. It's a bit like onboarding someone new, except this one can help write its own onboarding docs. Working with git helps too. If something doesn't feel right, I can always revert. That safety net makes it easier to experiment without worrying about breaking what already works.

I'm optimistic about where this is heading, and curious how my thinking will change as the tools keep evolving.
