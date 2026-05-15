+++
date = '2026-04-22T09:25:37-05:00'
draft = false
title = 'Self Learning'
+++
https://docs.google.com/document/d/1nzzsItmuu3anScEHo5nTgfloe13rnDAw117HdgLaP9Q/edit?tab=t.f7o757nux7cv

Describe the technology that the tutorial addresses and how it fits into your project.

The tech tutorial covered Vite as a modern frontend build tool and development server, specifically how it differs from tools like Create React App and Webpack. In our project, Vite handles the development server (npm run dev), the production (npm run build), and the API proxy configuration (vite.config.js) that routes /api calls from the browser to the Express server without CORS issues. It also provides React Fast Refresh, which keeps component state alive across new updates.

Evaluate how useful the tool was in your work. What were its strengths and weaknesses?

Vite was a strong fit for this project. Its cold-start time is nearly instant compared to Webpack-based setups, and the proxy configuration (four lines in vite.config.js) eliminated the CORS headache of running a separate API server in development. Its weakness is that configuration knowledge does not transfer well from older tooling — team members familiar with Create React App had to relearn where configuration lives and how plugins work. The ESM-first module system also caused friction with a few older require() patterns in early iterations of the server code.

Identify one particular sticking point to using this technology that you would want your team to know about when adopting the tool and how you resolved it.

The biggest sticking point was that Vite's dev server and the production build behave differently for asset imports. During development, importing local assets by path works fine, but the production build hashes asset filenames and moves them to dist/assets/. Our initial implementation used hardcoded /src/assets/grinnell-logo.png paths directly in JSX (e.g., src/App.jsx:651), which works in development but would break in a deployed production build. The correct fix is to import assets as ES modules (import logo from './assets/grinnell-logo.png') so Vite tracks and rewrites the path at build time. We did not fully resolve this before the semester ended, so production deployment would require this fix.