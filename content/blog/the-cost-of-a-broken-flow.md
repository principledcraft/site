---
title: The cost of a broken flow
date: 2026-03-03
summary: Recently it took me two hours to implement a change and more than two frustrating days to deploy it. It reminded me how much I appreciate when developer workflows are dialled in and effortless. The investment in getting them right earns itself back with interest.
authors:
  - name: PrincipledCraft
draft: false
---

{{< summary >}}

I finished the implementation in about two hours. One of those stretches where everything clicked and the code came together quickly. Then I tried to deploy it. What followed was more than two days of wrestling with infrastructure. Configuration that didn't match what I expected, pipelines failing for reasons unrelated to my change and a lot of copying of configuration for different environments. A setup that clearly hadn't been designed with a smooth developer experience in mind. By the end of it, the satisfaction from those productive two hours had completely evaporated. Two hours of implementation turned into two days of frustration.

It made me reconnect with my appreciation for good flow. A good flow means it's easy to add code, feedback is fast, and deployment is automated. You can do what you need to do quickly and effortlessly. You earn back the time you invested in setting up these processes. That's time you can spend making the code better, more maintainable and reliable, working on the next big ticket or helping others progress. It compounds in a way that's hard to appreciate until you've experienced both sides.

I think a lot of developers see this kind of work as secondary, something you do when you have spare time. But it's an enabler. Building and maintaining the processes around the code is just as much part of the job as writing the code itself. It can require complex infrastructure, and in my experience that investment pays for itself quickly. Even so, [simplicity](/principles/quality/simplicity) matters. Infrastructure that's a pain to use and update becomes its own bottleneck.

An hour spent improving [developer flow](/principles/quality/developer-flow) pays back many times over in unbroken concentration. It's one of the most undervalued investments a team can make.
