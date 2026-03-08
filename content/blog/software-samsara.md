---
title: Software samsara
date: 2026-03-08
summary: Software often gets rewritten when a team's composition changes or the original developers move on, an endless cycle of death and rebirth reminiscent of the Buddhist concept of samsara. But why does it happen, and how can we break the cycle?
authors:
  - name: PrincipledCraft
draft: false
---

{{< summary >}}

Who would have thought that studying religious sciences for a short while in college would help me better understand software engineering? In Buddhism, samsara is the cycle of death and rebirth, the soul moving from one life to the next, never quite arriving. Software has its own version: a team builds something, people leave, a new team inherits it, struggles to understand it, and eventually convinces the business to rewrite. Give it a few years and another team will be making the same case.

I've dealt with this myself. I joined a codebase and struggled to implement the simplest behaviours. The system had already been re-engineered from scratch because the previous one was impossible to comprehend. So why did history repeat itself so easily? [Peter Naur](https://en.wikipedia.org/wiki/Peter_Naur) nailed this in his 1985 paper [Programming as Theory Building](https://gwern.net/doc/cs/algorithm/1985-naur.pdf). The real product of programming isn't the code, but the theory the programmers hold in their heads: how the solution maps to the real world, why each part is the way it is, how it should respond to change. When those programmers leave, the theory dies with them.

> A dead program may continue to be used for execution in a computer and to produce useful results. The actual state of death becomes visible when demands for modifications of the program cannot be intelligently answered.<br>
> — <cite>Peter Naur</cite>

[Dan North](https://dannorth.net/) picks up on this in his talk *A Defence of Technical Excellence*: when the theory is gone and the codebase does not communicate its intent, confusion takes over. Confused teams slow down, produce more bugs, and deliver less.

{{< youtube BkxIwxSp7V0 >}}
<br />

So how do you keep the theory alive when people inevitably move on? [Simplicity](/principles/quality/simplicity) is the foundation, but it's not enough on its own. [Eric Evans](https://www.domainlanguage.com/) argues with Domain-Driven Design that the structure of your software should mirror the business problem it solves, with developers and domain experts sharing a common language. When your classes and modules are named after things the business actually talks about, the code itself carries the theory. [Richard Gabriel](https://en.wikipedia.org/wiki/Richard_P._Gabriel) calls this quality *habitability* in his essay "Habitability and Piecemeal Growth" from [Patterns of Software](https://archive.org/details/PatternsOfSoftware):

> Habitability is the characteristic of source code that enables programmers, coders, bug-fixers, and people coming to the code later in its life to understand its construction and intentions and to change it comfortably and confidently.<br>
> — <cite>Richard Gabriel</cite>

Habitable code is code you can feel at home in, where you can place your hands on any item without having to think deeply about where it is. Clear naming, sensible structure, and just enough in the code to communicate the *why* behind the decisions. Not a spec that will rot in a wiki, but enough to help someone build an accurate mental model.

The Buddha's answer to samsara wasn't to reject the world or to cling to it harder. It was the [Middle Way](https://en.wikipedia.org/wiki/Middle_Way): a path between extremes. The rewrite is one extreme, a dramatic rebirth that feels like progress but often just resets the clock. The other is resignation, letting the codebase rot because "it works". The Middle Way applied to software is keeping things simple enough that the next person can form a theory of how it works, rich enough that the domain is honestly represented. No rewrites, no neglect. Just steady, deliberate care.
