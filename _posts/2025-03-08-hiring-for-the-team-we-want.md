---
layout: post
title: Hiring for the Team We Want
tags: hiring
---

Recently, I participated in a discussion about revamping the technical interviews we conduct for Software Engineer candidates. It’s a deeply interesting topic, one we could discuss for hours, but there was a particular angle that gave me pause.

We were discussing the key competencies we should expect from candidates. Someone suggested that experience in observability and monitoring should be one of them. I completely agreed, given that our company already relies heavily on these practices, and they are part of the daily workflow for our technical teams. However, following the same logic, someone else proposed adding database query optimization to our requirements, as we’ve historically faced performance issues that we would like to improve—or at least not exacerbate.

That’s when my spider-sense tingled. I realized I would likely fail such a technical interview. I can barely grasp how an index works, and reading an `EXPLAIN` query plan gives me a headache. But behind this thought, a second one struck me: why has this knowledge never been critical for me, and why does it suddenly seem to be now?

Our need to optimize SQL queries arises from a legacy design that likely aligned with past business requirements but has since lagged behind as the company evolved. Classic technical debt. But if we want to bring in talent that will help us move forward strategically, we must be able to think beyond the boundaries of our present situation.

In our long-term technical vision—our North Star—we are already envisioning a shift toward an event-driven architecture that leverages projection generation in read models, helping us optimize data store queries to the fullest. If we make progress along this path, the need to optimize queries filled with `JOIN` and who-knows-what-else will become a thing of the past.

So the real question is: are we hiring for the team we are today, or for the team we want to become? If we only focus on solving today’s problems, we risk locking ourselves into the very constraints we’re trying to escape. Let’s build a team that doesn’t just patch the present but architects the future.