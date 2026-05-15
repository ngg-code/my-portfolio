+++
date = '2026-05-11T14:59:15-05:00'
draft = true
title = 'Quality'
+++
https://github.com/StudiousSquirrels/4yearplanner/blob/main/src/App.jsx
https://github.com/StudiousSquirrels/4yearplanner/blob/main/src/components/major_requirements.jsx
https://github.com/StudiousSquirrels/4yearplanner/blob/main/server/index.js

Where the codebase exemplifies engineering excellence:

Single source of truth for course validation. All course placement logic, including prerequisite checking, term offering restrictions, and enrollment caps, lives in one single function that is called by both the manual course selection path and the automatic fill algorithm. There is no duplicate validation code anywhere in the project, which means that fixing a bug or adding a new rule only ever needs to happen in one place and is guaranteed to apply everywhere the validation is used.

Normalized data access. Course codes in this project come from multiple sources and are not always formatted the same way. A code might appear as CSC-151 in one place, CSC151 in another, or even csc 151 in user input. A single normalization function is defined once and called everywhere course codes are compared, stripping out spaces, dashes, and converting everything to uppercase before any lookup is performed. Without this, course lookups would silently fail whenever the formatting happened to differ between the stored value and the value being searched for, producing bugs that would be very difficult to trace.

Parameterized SQL throughout. Every database query in the server uses placeholders rather than inserting user input directly into the query string. This is the standard defense against SQL injection attacks, where a malicious user could otherwise craft input that manipulates the database query in unintended ways. By using parameholders consistently, this entire class of vulnerability is eliminated.

Clean component boundaries. The two main visual components, the semester grid and the requirements panel, do not own or manage any of the data they display. They receive everything they need from the outside, display it, and report back when the user does something. This means their behavior is entirely predictable from the outside and they can be understood, tested, or replaced without needing to know anything about how the rest of the application works.

The sequencing algorithm avoids over-engineering. The problem of figuring out which courses to place in which semesters during the auto-fill process is related to a well-known computer science problem called topological sorting, which can be solved with a fairly complex graph traversal algorithm. Instead, the codebase solves this with a simple scoring function that estimates the right semester for a course based on its course number, treating 100-level courses as early and 400-level courses as late. This is not a perfect solution but it is entirely adequate for the scale of this problem and is much easier to understand and modify than a full graph-based approach would be.

Where the codebase falls short and how it would be addressed:

No tests. The most critical and complex logic in the project, including prerequisite validation, requirement block evaluation, and auto-fill sequencing, has no automated test coverage at all. This means that any change to the validation function could silently break course placement for specific majors and no automated system would catch it before a user encountered the problem. The fix would be to introduce a testing framework like Vitest and write unit tests that cover the key branches in the validation and evaluation functions, especially the edge cases around special-case requirement blocks and missing registration rules.

Hardcoded asset paths will break in production. The Grinnell logo and campus banner images are referenced using paths that only work in the local development environment. When the project is built for production, Vite processes and renames asset files, so those hardcoded paths will point to nothing and the images will fail to load. The fix is to import the image files as modules at the top of the component file, which tells Vite to track them and rewrite the paths correctly during the build process.

The requirement evaluation function is fragile under new majors. Currently, any major that has a special type of requirement block that does not fit the standard rule types requires a new hardcoded condition to be added directly inside the evaluation function. This works fine for a small number of majors but will become increasingly messy and error-prone as more majors are added to the system. The better long-term solution would be to store the special-case logic as structured data in the database alongside the requirement block itself, so the evaluation function can read the type of check it needs to perform rather than having to recognize specific block codes by name.

No partial loading states. If the server is slow to respond or a request fails, the entire interface shows a single loading message and nothing is usable until all the data arrives. A better experience would show the parts of the interface that are ready immediately, such as the major selector, while the course list and requirements panel load in separately. This could be achieved using skeleton placeholder components that give users a sense of the layout before the data fills in, reducing the feeling that the application is frozen or unresponsive.