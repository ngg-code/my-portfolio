+++
date = '2026-04-22T09:25:36-05:00'
draft = false
title = 'Architecture'
+++

**Medium-Scale Architecture**

 src/App.jsx (lines 34–82), server/index.js, src/components/semestersTable.jsx, src/components/major_requirements.jsx
```
  const [orGroupSelections, setOrGroupSelections] = useState({});

  useEffect(() => {
    async function loadPlannerData() {
      try {
        const [coursesResponse, majorsResponse] = await Promise.all([
          fetch("/api/courses"),
          fetch("/api/majors"),
        ]);

        if (!coursesResponse.ok || !majorsResponse.ok) {
          throw new Error("Could not load planner data from SQL.");
        }

        const [courses, majorsList] = await Promise.all([
          coursesResponse.json(),
          majorsResponse.json(),
        ]);

        setCoursesData(courses);
        setMajors(majorsList.majors);
      } catch (error) {
        setLoadError(error.message);
      }
    }

    loadPlannerData();
  }, []);

  useEffect(() => {
    async function loadMajorRequirements() {
      try {
        setLoadError("");
        setMajorRequirements(null);

        const requirementsResponse = await fetch(
          `/api/majors/${selectedMajorCode}/requirements`,
        );

        if (!requirementsResponse.ok) {
          throw new Error("Could not load major requirements from SQL.");
        }

        setMajorRequirements(await requirementsResponse.json());
      } catch (error) {
        setLoadError(error.message);
      }
    }

    loadMajorRequirements();
  }, [selectedMajorCode]);
```
What architectural pattern does your program employ?

The project is split into two separate sides that communicate by passing data back and forth. The backend acts like a waiter that only knows how to answer specific questions and always responds with a structured list of information. The frontend is the part the user actually sees and interacts with, and whenever it needs information it sends a request to the backend, gets the data back, and uses it to update what is shown on screen. Neither side needs to know how the other works internally. The backend does not care how the frontend displays the courses, and the frontend does not care how the backend retrieves them from the database. They just agree on what the data looks like when it is passed between them.

What components result from this pattern in your program?

Four distinct layers emerged from this pattern. The first is the database layer, which stores all the course information, the list of majors, and the specific requirements for each major. The second is the API layer, which sits between the database and the frontend and is responsible for pulling the right information out of the database and sending it to the frontend in a clean, readable format. The third is the state layer, which is the brain of the frontend and keeps track of everything the user is doing, including which courses are placed in which semesters, what major is selected, and whether the plan has been checked against requirements. The fourth is the presentation layer, which contains the visual components the user actually sees and interacts with. These components do not make any decisions on their own and instead simply display whatever data they are given and report back to the state layer when the user does something like clicking a course slot or expanding a requirement block.

What technologies and/or libraries make up each of the components?

The database layer uses PostgreSQL as the database itself, with a Node.js library called pg acting as the bridge that allows the server code to send queries to the database and get results back. The API layer is built with Express, a lightweight framework for handling incoming requests and sending responses. The state and presentation layers are both built with React, using built in tools called hooks that let the components manage and respond to changing data over time. The entire project is bundled and served during development using Vite, which also handles a small but important detail: when the frontend makes a request to the backend during development, Vite automatically forwards that request to the correct local server address so the two sides can communicate without any extra configuration. Finally, ESLint runs across all the code files and flags problems like incorrectly written hooks or unused variables before they cause bugs at runtime.