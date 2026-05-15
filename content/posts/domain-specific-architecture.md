+++
date = '2026-05-11T14:52:52-05:00'
draft = false
title = 'Domain Specific Architecture'
+++
src/App.jsx lines 160–308 (getPreferredSemesterIndex, getSequenceScore, canPlaceCourse)
https://github.com/StudiousSquirrels/4yearplanner/blob/main/src/App.jsx

What aspects of your software architecture arise because of the domain?

The entire course placement layer exists solely because of the domain: academic scheduling has prerequisites (a directed dependency graph), term offerings (courses only exist in Fall or Spring), department enrollment caps (no more than 2 CSC courses/semester in years 1–2), and standing requirements. None of these constraints map onto generic CRUD patterns. The architecture adds a dedicated constraint validation layer (canPlaceCourse, lines 233–308) that sits between any user or algorithm action and the state mutation. Every course placement — manual or automated — passes through this layer before anything changes.

What domain-specific problem does your architectural choice solve?

The central domain problem is that academic prerequisites form a directed acyclic graph (DAG): a course can only be placed if all its predecessors appear in earlier semesters. This is fundamentally different from a standard form-input problem. Placing CSC-301 requires that CSC-207 and CSC-208 are both already in earlier slots, and those courses themselves have their own predecessors. Without an explicit validation layer, the auto-fill algorithm and the manual selection path would each need to duplicate prerequisite-checking logic, and omitting it would silently produce illegal plans.

How does your solution solve these issues?

canPlaceCourse encodes all domain constraints in one place and returns a boolean. The autoFillPlan algorithm (lines 513–633) iterates slots in chronological order and calls canPlaceCourse for each candidate — the temporal iteration implicitly enforces topological ordering because a course can only be placed once its prerequisites have already been placed in earlier (already-processed) slots. The getSequenceScore function (lines 181–189) then adds a scoring layer that approximates the preferred depth in the DAG using course level numbers (100s → early, 400s → late), producing plans that feel natural without explicitly walking the graph.