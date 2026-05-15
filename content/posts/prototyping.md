+++
date = '2026-04-22T09:25:37-05:00'
draft = false
title = 'Prototyping'
+++
https://docs.google.com/document/d/1nzzsItmuu3anScEHo5nTgfloe13rnDAw117HdgLaP9Q/edit?tab=t.f7o757nux7cv

What design questions did you intend to answer with your prototypes?

Our prototypes were built around two core questions. The first was whether organizing the plan as a grid of semesters would feel natural to students, or whether it would require explanation before they could use it confidently. The second was whether showing requirement completion status updating in real time as courses were added would help students plan more effectively, or whether it would pull their attention away from the task of actually building their schedule and cause them to make worse decisions.

How did you design your prototypes and subsequent user studies to address these questions?

To test the grid layout, we built a paper prototype that arranged the eight semesters across the page in four columns, with each column representing one academic year split into its fall and spring semesters. Three students were asked to sit with the prototype and fill in a sample plan while thinking out loud, so we could hear what they were interpreting and where anything felt unclear. To test the live versus on-demand question, we built two working versions of the requirements panel. One version updated automatically every time a course was added to the plan, while the other only showed completion status when the user explicitly clicked a button to check their progress. We alternated which version each participant saw first to avoid any bias from ordering, and we watched closely for moments where their attention shifted away from the planning grid toward the requirements panel.

What answers to your design questions did your efforts unveil?

The semester grid turned out to be immediately understandable to all three students without any explanation needed. Each one began filling in courses right away and moved through the semesters in a natural order without hesitation. The live updating requirements panel told a different story. Two out of three students who saw it began paying close attention to the requirements checklist after adding just a few courses, and as a result started making course choices based on what would turn the most checkboxes green rather than what made sense for their sequence and workload. This led to plans that satisfied requirements on paper but had courses placed in an order that did not respect prerequisites or a sensible academic progression. The on-demand version kept students focused on building a coherent plan first and checking requirements second, which produced much better outcomes. This confirmed that the final design should use the explicit check button rather than live updates.
