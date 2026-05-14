+++
date = '2026-04-22T09:25:37-05:00'
draft = true
title = 'Infrastructure'
+++

What development tools did you use through the process to help write code and automate the build/deployment process?

The primary development tools were: Vite as the build tool and dev server (with hot module replacement making UI changes instantly visible without a full reload). Node.js + Express for the local API server, PostgreSQL for the database with SQL seed scripts loaded via psql, and Git + GitHub for version control and PR-based code review. npm run server and npm run dev are the two commands that together bring the full stack up locally.

What is one specific problem you encountered while writing code and how did your tools help or hinder your ability to address that problem?

When implementing the major-switch feature (swapping from CSC to Anthropology requirements), the requirements panel was rendering stale data — the old major's blocks appeared briefly after the dropdown changed. Vite's hot reload let us iterate on the fix quickly, but the root cause was an ESLint react-hooks/exhaustive-deps warning we had initially suppressed. Once we paid attention to it and added selectedMajorCode to the useEffect dependency array in src/App.jsx (line 61), the stale render disappeared. ESLint's warning was the diagnostic that pointed directly at the bug.