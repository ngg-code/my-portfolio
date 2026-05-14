+++
date = '2026-05-11T14:59:15-05:00'
draft = false
title = 'Quality'
+++

Where the codebase exemplifies engineering excellence:

Single source of truth for course validation. 

canPlaceCourse (src/App.jsx:233–308) is called by both the manual selection path and the auto-fill algorithm. There is no duplicate prerequisite-checking code anywhere.
Normalized data access. normalizeCourse is defined once and used consistently everywhere course codes are compared — it handles the CSC-151 vs CSC151 vs csc 151 variations that arise from different data sources. Without it, course lookups would silently fail on format mismatches.
Parameterized SQL throughout. Every query in server/index.js uses $1/$2 placeholders, eliminating SQL injection as a vulnerability class.
Component boundaries are clean. SemestersTable and MajorRequirements are fully controlled components: they own no authoritative state, accept data via props, and communicate upward through callbacks. This makes their behavior completely predictable from the outside.
The sequencing algorithm avoids over-engineering. getSequenceScore (lines 181–189) solves the topological ordering problem with a simple function rather than a full graph traversal — adequate for the domain size and much easier to reason about.

Where it falls short and how you would address it:

No tests. The most critical logic — prerequisite validation, requirement block evaluation, auto-fill sequencing — has zero test coverage. Any change to canPlaceCourse can silently break plans for specific majors. Fix: add Vitest unit tests.
Hardcoded asset paths will break in production. /src/assets/grinnell-logo.png (App.jsx:651) uses a dev-server path rather than an imported asset reference. Fix: import logo from './assets/grinnell-logo.png' so Vite tracks and rewrites it at build time.
Error messages expose internal details. res.status(500).json({ error: error.message }) in server/index.js leaks PostgreSQL error text. Fix: return a generic message to clients and log the full error server-side only.
getBlockStatus is fragile under new majors. Adding a new major with a special-case requirement block requires modifying getBlockStatus with a new if (block.code === ...) branch. This will scale poorly as more majors are added. Fix: encode special-case logic as data in the database (e.g., a block_type field) so the function can dispatch by type rather than by hard-coded block code string.
No loading/error state for individual semesters. If the API is slow, the entire UI shows "Loading..." with no partial rendering. Fix: use React Suspense or skeleton placeholders per section so the major selector is usable before the full course list loads.