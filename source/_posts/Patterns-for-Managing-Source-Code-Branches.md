---
title: Patterns for Managing Source Code Branches
date: 2020-09-07 20:13:11
tags: ["learning"]
categories:
- ["Learning","Articles"]
---
Recently, I finished reading the article [Patterns for Managing Source Code Branches](https://martinfowler.com/articles/branching-patterns.html#FinalThoughtsAndRecommendations>) and was able to learn new stuff and provide another way for me to think on CI/CD related stuff.
<!-- more -->
The article did not touch much about Continuous Delivery/Deployment but on the patterns for managing source code branches.
The important takeaway for me was:

- there is no straightforward pattern that can fit your team or your product. We can however make tweaks to the branching model with patterns such as *maturity branches* , *experimental branches* etc...
- attempting to force a pattern meant for a single release in production onto a multiple release in production product can be confusing and increase complexity.
- we should look at ways to reduce [Integration Friction](https://martinfowler.com/articles/branching-patterns.html#integration-friction) and in the process strive to achieve [High-Frequency Integration](https://martinfowler.com/articles/branching-patterns.html#High-frequencyIntegration)
