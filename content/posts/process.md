+++
date = '2026-04-22T09:25:37-05:00'
draft = true
title = 'Process'
+++

What software engineering practices did your group employ to manage, distribute, and execute on their work?

The team used GitHub as the central hub for all collaboration. All development work happened on separate feature branches so that changes could be reviewed and tested before being brought into the main codebase. When a feature was ready, it was submitted as a pull request that required at least one teammate to look over before merging. This gave everyone visibility into what was changing and created a natural checkpoint to catch problems before they affected the shared codebase. Work was also distributed by area of ownership so that teammates could move quickly without constantly stepping on each other. Linda Jing was primarily responsible for the SQL schema and the data that powered the backend, Drilon Qerimi focused on the backend server and its connection to the database, and I was primarily responsible for the requirements panel component and the overall visual design of the application including the branded header, campus banner, and the collapsible tree layout.

In your estimation, how well did your group follow through on using these processes throughout the semester?

The pull request and review process was followed consistently for the larger and riskier integrations, such as when the SQL backed data was first connected to the frontend and when the requirement logic was merged in. These were the moments where the codebase was most likely to break if something was misaligned, so the team naturally treated them with more care. For smaller and more incremental changes, particularly toward the end of the semester when deadlines were close, the process was less strictly followed and some changes were pushed directly to main without going through a formal pull request. Overall the team used the process well when it mattered most but loosened up on it as time pressure increased.

What, if anything, got in the way of integrating these processes into your workflow?

The biggest obstacle was the local database setup that the project required. Because the application depends on a running PostgreSQL instance with all the seed data loaded in, reviewing a teammate's changes was not as simple as pulling their branch and opening the app. Any change to the SQL files meant the reviewer also had to wipe and reload their local database before they could see the effects in the frontend, which added a meaningful amount of friction to the review process. In practice this meant that some database changes were merged after reviewing the SQL code alone rather than after fully running and testing the application end to end. A shared staging environment or a script that could reset and reseed the database in one step would have made it much easier to follow the review process consistently throughout the semester.