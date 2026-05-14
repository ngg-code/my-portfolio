+++
date = '2026-04-22T09:25:37-05:00'
draft = false
title = 'Process'
+++

What software engineering practices did your group employ to manage, distribute, and execute on their work?
 
The team used GitHub flow: all work happened on feature branches, changes were submitted as pull requests, and merges required a review before being merged into main. Work was distributed by component ownership — Linda Jing was primarily responsible for SQL schema, Drilon Qerimi focused on the backend, and I was primarily responsible for the MajorRequirements component UI and the overall visual design of the app.

In your estimation, how well did your group follow through on using these processes throughout the semester?

The PR-and-review process was followed for the majority of last larger integrations , which were the riskiest points where divergent codebases needed to merge. For smaller changes — particularly the UI work like commit 3dbb7d3, code was pushed directly to main without a PR. Overall, the team used the process well when the stakes were high but let it slip on later incremental work as we got close to the end of the semester.

What, if anything, got in the way of integrating these processes into your workflow?

The main friction was the local database setup requirement. Because the project requires a running PostgreSQL instance with the seed data loaded, we could not always review and run each other's code quickly. We reviewed a SQL schema change had to reload the database before they could verify the behavior in the UI, which added enough friction that some changes were merged on inspection of the SQL alone rather than after a full end-to-end test.    s