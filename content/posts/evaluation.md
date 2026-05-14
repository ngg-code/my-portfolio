+++
date = '2026-04-22T09:25:37-05:00'
draft = false
title = 'Evaluation'
+++

What was the design of your user study to evaluate your artifact?

After building the working application, we ran a structured usability test with three Grinnell students who had not seen the tool before. Each participant was given the same task: "Plan a four-year schedule for a CS major, starting from scratch, and verify that your plan satisfies major requirements." Afterward we asked a short questions about what was confusing and what they would change.

What were the results of this user study?

All three users successfully completed a plan and reached the requirements check. The main friction points were: (1) users initially did not notice the "Auto-Fill Remaining Plan" button because it was visually similar to the other control buttons; (2) the error message when placing a course that failed a prerequisite check was visible but users sometimes dismissed it quickly and lost track of why a slot remained empty. One user suggested color-coding the semester cards differently for years to aid orientation.

What were the main points of future development that you took away from this activity?

The primary future work items were: (1) visually distinguish the Auto-Fill button to make it more discoverable; (2) show inline slot-level error indicators rather than relying on a dismissable banner message for prerequisite violations; (3) optionally color-code or group semester cards by year. A longer-term suggestion was to allow students to save and share their plans.