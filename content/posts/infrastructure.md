+++
date = '2026-04-22T09:25:37-05:00'
draft = false
title = 'Infrastructure'
+++

What development tools did you use through the process to help write code and automate the build/deployment process?

The primary development tools were: Vite as the build tool and dev server (with hot module replacement making UI changes instantly visible without a full reload). Node.js + Express for the local API server, PostgreSQL for the database with SQL seed scripts loaded via psql, and Git + GitHub for version control and PR-based code review. npm run server and npm run dev are the two commands that together bring the full stack up locally.

What is one specific problem you encountered while writing code and how did your tools help or hinder your ability to address that problem?

When the major switching feature was being built, a bug appeared where switching from one major to another in the dropdown would briefly show the old major's requirement blocks before updating to the new ones. The immediate instinct was to dig into the component rendering logic and check whether the data fetching was being triggered at the right time, which led to a fair amount of manual testing by switching between majors repeatedly and watching what happened on screen. Vite's hot reload was genuinely useful during this process because every small change to the code was reflected in the browser almost instantly without needing to manually refresh the page or restart anything, making it much faster to try different approaches and see their effects in real time.

The actual root cause turned out to be something ESLint had already flagged but that had been dismissed at the time. There is a rule in ESLint specifically for React hooks that warns whenever a variable is used inside a hook but is not listed in the dependency array, which is the list that tells React when to re-run the hook. The major code variable was being used inside the data fetching hook to determine which major's requirements to load, but it had not been added to the dependency array, meaning React had no idea it needed to re-run the fetch when the major changed. Once the warning was taken seriously and the variable was properly added, the stale data issue disappeared entirely. The key takeaway was that ESLint warnings, especially ones related to React hooks, are rarely just style suggestions and in this case the warning was pointing directly at a real behavioral bug that would have been much harder to find through manual debugging alone.