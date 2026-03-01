---
title: The Cost of Poor Quality
type: docs
weight: 2
---

Poor code quality is not just a developer inconvenience. It is a business problem with measurable consequences. Teams that let quality slide write software more slowly, less predictably, and with more defects.

## Most of the cost is maintenance

Estimates vary, but research consistently puts maintenance at [50% to 80% of total software cost](https://idealink.tech/blog/software-development-maintenance-true-cost-equation). IBM puts it at 50-75%, Gartner at 55-80%. The Standish Group found that enhancements and modifications after initial deployment typically cost [three to four times the original development](https://llinformatics.com/blog-posts/initial-cost-vs-lifetime-cost-of-software-development). The code you write today will be read and changed far more often than it was written. That alone makes a strong case for writing code that is easy to understand and safe to change.

## The evidence

[Adam Tornhill](https://adamtornhill.com/) and [Markus Borg](https://en.wikipedia.org/wiki/Markus_Borg) studied 39 proprietary production codebases in their paper [Code Red: The Business Impact of Code Quality](https://arxiv.org/abs/2203.04374). Their findings are hard to ignore:

- Code with quality problems has **15 times more defects** than healthy code
- Implementing a task in unhealthy code is **124% slower** than in healthy code. A feature that could ship in one week takes more than two
- The worst part is **unpredictability**: the maximum time to complete a task in unhealthy code is an order of magnitude larger than in healthy code. Planning becomes guesswork

When someone argues that "we don't have time for quality," they are arguing for exactly those outcomes.

## Broken windows

[The Pragmatic Programmer](/books/the-pragmatic-programmer/) borrows a concept from criminology: the broken window theory. A building with one broken window, left unrepaired, signals that nobody cares. More windows get broken. Neglect spreads.

Codebases work the same way. A poorly written module or a test suite that everyone knows is unreliable signals that low quality is acceptable here. New code written nearby tends to match the surrounding standard. If that standard is low, quality keeps eroding.

The opposite is also true. When a codebase is well-maintained, developers feel responsibility to keep it that way. Standards are contagious in both directions.

## Technical debt is a business concept

[Ward Cunningham](https://en.wikipedia.org/wiki/Ward_Cunningham) originally used the term "technical debt" as a metaphor for business people: shipping imperfect code is like taking on debt. It lets you move faster now, but you pay interest on it until you pay it back.

The metaphor is useful, but it gets misused. Debt implies a deliberate, strategic choice. Much of what gets called technical debt is not deliberate at all. It is the accumulated result of shortcuts and neglect. A more honest term for that might be *code rot*.

Whether deliberate or accidental, the effect is the same: every change takes longer and carries more risk. The interest payments are invisible on any single day, but they compound. Teams often do not notice the slowdown because it happens gradually, until a change that should take a day takes a week.