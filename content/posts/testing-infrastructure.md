+++
date = '2026-04-22T09:25:36-05:00'
draft = false
title = 'Testing Infrastructure'
+++

https://github.com/ngg-code/4yearplanner/blob/main/vite.config.js
https://github.com/ngg-code/4yearplanner/blob/main/package.json
https://github.com/ngg-code/4yearplanner/blob/main/src/test/setup.js

How have you automated your tests so that they both take one click to run and run automatically as part of build validation?

The testing infrastructure is built on Vitest, which plugs directly into the existing Vite build setup through vite.config.js. A test block was added to that file specifying jsdom as the rendering environment and pointing to a setup file at src/test/setup.js that imports @testing-library/jest-dom to enable matchers like toBeInTheDocument. Two scripts were added to package.json: npm run test starts Vitest in watch mode, where it automatically re-runs all affected tests every time a file is saved, giving immediate feedback during development without any manual action. npm run coverage runs the full suite once and prints a coverage table broken down by statements, branches, functions, and lines for every file in the components folder. Because Vitest shares its configuration with Vite, there is no duplication between the build config and the test config and both tools stay in sync automatically.

Describe a specific occurrence in which your testing infrastructure saved you and/or your team work?

The specific occurrence where this infrastructure would have prevented real lost work is the stale major data bug described in the infrastructure section. When the major switching feature was being built, the requirements panel was showing the old major's blocks after the dropdown changed because a variable was missing from a hook's dependency array. At the time this was caught through manual testing, which required repeatedly switching between majors and watching the screen carefully. If a test had existed that rendered the component with one major's requirements, passed in a new major's requirements via updated props, and then checked that the new block titles appeared and the old ones did not, that test would have failed immediately and pointed directly at the problem without any manual investigation. The watch mode in Vitest would have caught this the moment the file was saved, before the bug was ever merged into the shared codebase.