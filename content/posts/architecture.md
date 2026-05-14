+++
date = '2026-04-22T09:25:36-05:00'
draft = false
title = 'Architecture'
+++

**Medium-Scale Architecture**

 src/App.jsx (lines 34–82), server/index.js, src/components/semestersTable.jsx, src/components/major_requirements.jsx

What architectural pattern does your program employ?

The project uses a REST API + Single Page Application (SPA) pattern. The backend exposes a stateless JSON API over HTTP; the frontend is a React SPA that fetches from those endpoints and manages all interactive state locally. This is a client-server separation where neither side knows about the other's internal structure — the server just returns JSON, and the client just consumes it.

What components result from this pattern in your program?

Four distinct layers emerged: (1) a database layer (PostgreSQL) storing courses, majors, and per-major requirement blocks across 20+ SQL seed files; (2) an API layer (server/index.js) with three Express routes (/api/courses, /api/majors, /api/majors/:code/requirements) that query the database and return structured JSON; (3) a state layer (App.jsx) that owns all shared client state — semesters, coursesData, majorRequirements, checkedSemesters — and contains the course placement, validation, and auto-fill logic; (4) a presentation layer (SemestersTable, MajorRequirements) consisting of pure components that receive data via props and emit events upward.

What technologies and/or libraries make up each of the components?

The database layer uses PostgreSQL with the pg Node.js client library. The API layer runs on Express 5. The state layer and presentation layer are React 19 with hooks (useState, useEffect, useMemo). The build and development toolchain is Vite 8 with @vitejs/plugin-react, which also proxies /api requests to localhost:3001 during development (configured in vite.config.js, line 6–8). ESLint 9 with eslint-plugin-react-hooks enforces code quality across all JavaScript files.