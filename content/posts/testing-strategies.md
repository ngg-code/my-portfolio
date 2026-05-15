+++
date = '2026-04-22T09:25:36-05:00'
draft = false
title = 'Testing Strategies'
+++

Sample: src/components/major_requirements.test.jsx
https://github.com/ngg-code/4yearplanner/blob/main/src/components/major_requirements.test.jsx

According to code coverage tools, what aspects of this component are covered with tests?

The component I was responsible for testing is MajorRequirements. Running npm run coverage produced the following results for major_requirements.jsx: 54% of statements, 56% of branches, and 60% of functions are covered. The tests cover the following aspects of the component: the initial unchecked state where all indicators are neutral and the hint message is visible, the evaluation of must_take blocks where the required course must be present in the plan, choose_one blocks where any single option satisfies the requirement, choose_n blocks where a minimum number of courses must be planned, and the special ANTH_FOUR_FIELDS block that checks breadth across academic subfields rather than a simple course count. The collapsible tree interaction is also covered, verifying that clicking a block row hides and reveals its course list independently of other blocks, and that course names and notes render correctly.

What kinds of tests are they?

The tests are component integration tests rendered into a real browser-like environment using React Testing Library and Vitest. Rather than calling internal functions directly, each test renders the full component with a specific set of props and then queries the output for text and visible elements the same way a user would experience them. For example, a test for the must_take block renders the component with a plan containing CSC-151 and checks that a checkmark appears in the output. A test for the collapsible behavior clicks a block row title and checks that the course list beneath it disappears from the screen. This approach validates the component as a whole rather than its individual pieces in isolation.

For code that is not covered by tests, why did you not cover them?

The uncovered portions are the CSC_ELECTIVE, CSC_MATH_ELECTIVE, and CSC_TOTALS_AND_POLICIES special cases inside getBlockStatus. These were left out because they require a significantly more complex set of mock course data to test accurately, including courses with specific credit counts, department prefixes, and elective eligibility rules that interact with each other in ways that take considerable setup to reproduce correctly. The SemestersTable component also has zero coverage because it was built primarily by a teammate and was not part of my testing responsibility. Given more time, the elective logic would be the highest priority area to cover next since it contains the most branching and is the most likely source of silent bugs when new majors are added.