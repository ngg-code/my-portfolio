+++
date = '2026-05-15T06:32:11-05:00'
draft = true
title = 'Ideation'
+++

What techniques did you use to discover solutions to users' needs?

After gathering insights from the needfinding interviews, I focused on the two biggest pain points that came up consistently: not knowing whether a course sequence was valid and having to manually cross-reference the catalog to check major progress. I used these as anchors for sketching out possible solutions, evaluating each idea by asking whether it actually addressed those frustrations or just added complexity without solving the core problem. 

What solution or solutions did you ultimately pursue and why did you choose them?

The solution I focused on was designing the requirements panel as a collapsible tree structure, where each requirement block can be expanded to reveal the individual courses that satisfy it and collapsed once complete to keep the view focused on what still needs attention. This was chosen over simpler alternatives like a flat checklist because it captures something a list fundamentally cannot represent, which is the hierarchical relationship between a requirement block and its constituent courses. A student looking at a flat checklist can see whether a block is done but cannot immediately see which specific courses are missing or which ones they have already planned toward it. The tree makes that information available at a glance without overwhelming the screen, since completed blocks can be collapsed away entirely. The on-demand approach, where the tree only shows completion status after the student clicks a check button rather than updating live, was also a deliberate design choice made after prototyping revealed that constantly shifting status indicators distracted students from the planning task itself.