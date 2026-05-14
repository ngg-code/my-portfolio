+++
date = '2026-04-22T09:25:36-05:00'
draft = false
title = 'Abstraction'
+++

src/components/major_requirements.jsx, lines 57–123 (getBlockStatus) and lines 139–146 (block evaluation pipeline)

What functionality does your code sample abstract away?

getBlockStatus() abstracts away all the logic for determining whether a major requirement block has been satisfied. Callers never need to know what rule type a block is (must_take, choose_one, choose_n, choose_credits, or special cases like ANTH_FOUR_FIELDS). They simply pass in a block object and receive back a uniform { completed, completedCourses, completedCredits, detail } result.

How does it mechanically achieve abstraction?

The function is a single entry point that branches internally by block.code and block.ruleType. For the ANTH_FOUR_FIELDS case (lines 66–76), it iterates over planned courses, collects unique subfield tags from the course data, and returns completion when at least three subfields are covered — a calculation completely unlike the standard course-count checks for must_take or choose_n. All branches return the same shaped object, so the output is predictable regardless of what path was taken internally. The rendering layer at lines 213–354 consumes only status.completed and status.detail and never inspects ruleType or course arrays directly.

What purpose does this abstraction serve in your larger program?

The abstraction enforces a clean boundary between evaluation logic and display logic. Adding a new rule type only requires adding a branch inside getBlockStatus — the tree-rendering JSX and the progress bar percentage calculation at lines 148–150 require no changes. It also makes the MajorRequirements component easy to reuse across different majors: swapping from CSC to Anthropology to Economics only changes the data passed in, not the evaluation code.