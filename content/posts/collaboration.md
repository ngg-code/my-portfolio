+++
date = '2026-04-22T09:25:37-05:00'
draft = false
title = 'Collaboration'
+++

Instance 1 — Code Review of a Pull Request

Linda opened a pull request to add the requirement logic to the SQL files and connect it to the main application file. During the review, an issue was identified where the prerequisite logic did not account for a course appearing in both a required block and an elective block at the same time. Without handling this case, the same course could count toward two separate requirements simultaneously, which would allow a student's plan to appear complete when it actually was not. The review comment was addressed and Linda pushed an updated commit that corrected the logic.

Instance 2 — Filing a Pull Request

This pull request represented a major overhaul of the requirements panel, switching it from a flat list to a collapsible tree view, and also added the Grinnell branded header and campus banner image to the application. The main difficulty was coordinating the visual changes with the data structure that Linda had built on the backend. The tree view relied on each requirement block having a consistent set of fields in the response coming from the server, so before the UI work could be finalized, it was necessary to confirm with Linda exactly what field names and structure to expect. Only once that was agreed upon could the component be written with confidence that it would work with the real data.

Instance 3 — Resolving a Bug

After the SQL backed data was merged into the project, a bug appeared where the application would crash when trying to place certain courses. The problem was that some majors did not have any registration rules stored in the database, so when the course placement function tried to read a property off the registration rule, it found nothing there instead of the expected value and threw a runtime error. The fix involved adding a safety check so the function could handle the case where no registration rule existed at all. The bug was particularly tricky to track down because it only showed up for specific majors that happened to have no registration rules, making it appear and disappear inconsistently depending on which major was selected.

Instance 4 — Merge Conflict Resolution (commit 95b6b0c)

Before pull request three was opened, Drilon merged the main branch into the feature branch to bring it up to date. This caused a conflict in the main application file because both branches had independently made changes to overlapping parts of the code. The main branch had updated how the semester slots were initially set up, while the feature branch had changed how courses were looked up by their code. Resolving the conflict required carefully reading both versions of the code, understanding the intent behind each change, and then deciding which parts of each version to keep. This involved direct coordination with both Linda and Drilon to make sure the final merged version preserved all the intended behavior from both sides.