+++
date = '2026-05-11T14:53:54-05:00'
draft = false
title = 'Testing Technologies'
+++

https://github.com/ngg-code/4yearplanner/blob/main/src/components/major_requirements.test.jsx

What advanced testing tool or technology did you employ in your code?

The testing suite uses two tools working together: Vitest as the test runner and React Testing Library as the component testing framework. Vitest is a modern testing framework built specifically for projects that use Vite, which means it shares the same configuration and module resolution as the rest of the project without needing a separate build pipeline for tests. React Testing Library is a library designed around the principle that tests should interact with components the same way a real user would, by looking for visible text, headings, and elements on screen rather than poking at internal state or implementation details. The two tools are installed as development dependencies in package.json and configured through vite.config.js and the setup file at src/test/setup.js.

What did this tool or technology validate in your program?

Together these tools validated the rendered behavior of the MajorRequirements component across a wide range of scenarios. On the rendering side, they confirmed that the component correctly displays the major name as a heading, shows the right progress badge count, renders all block titles and their rule labels, and shows the hint message telling users to click Check Requirements before any plan has been submitted. On the evaluation side, they validated that the must_take logic correctly marks a block complete only when the exact required course appears in the planned semesters, that choose_one marks a block complete when any one of its listed options is planned, that choose_n only marks a block complete once the minimum number of courses is reached, and that the special ANTH_FOUR_FIELDS block correctly counts distinct academic subfields rather than individual courses, showing the detail text like 2 / 3 subfields or 3 / 3 subfields depending on how many subfields the planned courses cover. On the interaction side, they validated the collapsible tree behavior, confirming that clicking a block row hides its course list, clicking it again brings the list back, and that collapsing one block has no effect on any other block in the tree.

How effective was it in this task?

React Testing Library was particularly effective because it treats the component as a black box, which means the tests do not break when internal implementation details change as long as the visible output remains correct. For example, if the internal variable names inside getBlockStatus were renamed or the function was restructured, none of the tests would need to change as long as the checkmark still appears when the right courses are planned and the progress badge still shows the right count. This made the tests genuinely useful as a safety net rather than as fragile assertions that would break every time the code was touched. The jsdom environment used by Vitest also made it possible to test the click interaction on the collapsible rows without needing a real browser, which kept the tests fast and self-contained. The coverage tool that comes bundled with Vitest gave a precise breakdown of which branches inside the component were and were not exercised, making it easy to see exactly which special cases still needed test coverage and prioritize where to focus next.

In what contexts did you envision yourself utilizing this tool or technology in the future?

React Testing Library would be the first choice for testing any component that has meaningful conditional rendering, state-driven visibility changes, or user interaction logic. Any time a component shows different content depending on what data it receives or what the user has clicked, this tool can verify those behaviors quickly and reliably without coupling the tests to how the component is implemented internally. Vitest specifically would be a natural fit for any future project that uses Vite as its build tool, since the shared configuration removes the overhead of maintaining a separate test setup. Beyond React projects, Vitest can also run plain JavaScript unit tests, so it could cover pure logic functions like the course validation and sequencing algorithms that currently have no test coverage in this project.