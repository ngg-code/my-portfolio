+++
date = '2026-04-22T09:25:36-05:00'
draft = false
title = 'Abstraction'
+++

src/components/major_requirements.jsx, lines 57–123 (getBlockStatus) and lines 139–146 (block evaluation pipeline)
https://github.com/StudiousSquirrels/4yearplanner/blob/main/src/components/major_requirements.jsx

```
  function getBlockStatus(block) {
    const completedCourses = block.courseCodes.filter((c) =>
      plannedCourses.has(normalizeCourse(c)),
    );
    const completedCredits = completedCourses.reduce(
      (t, code) => t + (courseMap[normalizeCourse(code)]?.credits || 0),
      0,
    );

    if (block.code === "ANTH_FOUR_FIELDS") {
      const usedSubfields = new Set();
      const usedCourses = [];
      for (const code of plannedCourses) {
        const course = courseMap[code];
        if (!course || !code.startsWith("ANT") || getCourseNumber(code) < 200) continue;
        for (const sf of course.subfields || []) usedSubfields.add(sf);
        if (course.subfields?.length) usedCourses.push(code);
      }
      return { completed: usedSubfields.size >= 3, completedCourses: usedCourses, completedCredits: usedCourses.length * 4, detail: `${usedSubfields.size} / 3 subfields` };
    }

    if (block.code === "ANTH_ADV_PATH_B") {
      const hasThesis = plannedCourses.has("ANT499");
      const adv = Array.from(plannedCourses).filter((c) => c.startsWith("ANT") && getCourseNumber(c) >= 300);
      return { completed: hasThesis && adv.length >= 1, completedCourses: [...adv, ...(hasThesis ? ["ANT499"] : [])], completedCredits: adv.length * 4 + (hasThesis ? 4 : 0) };
    }

    if (block.code === "CSC_ELECTIVE") {
      const requiredCodes = getUsedRequiredCourses();
      const usedElectives = [];
      let credits = 0;
      for (const code of plannedCourses) {
        const course = courseMap[code];
        if (!course || !code.startsWith("CSC")) continue;
        if (requiredCodes.has(code) || ["CSC281", "CSC282"].includes(code) || getCourseNumber(code) < 200) continue;
        credits += code === "CSC326" ? Math.min(course.credits || 0, 2) : course.credits || 0;
        usedElectives.push(code);
      }
      const sys = ["CSC211", "CSC213"].filter((c) => plannedCourses.has(c));
      if (sys.length === 2) {
        const extra = sys.find((c) => !usedElectives.includes(c));
        if (extra) { credits += courseMap[extra]?.credits || 0; usedElectives.push(extra); }
      }
      return { completed: credits >= (block.minCredits || 4), completedCourses: usedElectives, completedCredits: credits, detail: `${credits} / ${block.minCredits || 4} cr` };
    }

    if (block.code === "CSC_MATH_ELECTIVE") {
      const used = Array.from(plannedCourses).filter(
        (c) => (c.startsWith("MAT") && getCourseNumber(c) > 131) || c.startsWith("STA"),
      );
      return { completed: used.length >= 1, completedCourses: used, completedCredits: used.reduce((t, c) => t + (courseMap[c]?.credits || 0), 0) };
    }

    if (block.code === "CSC_TOTALS_AND_POLICIES") {
      const used = Array.from(plannedCourses).filter(
        (c) => (c.startsWith("CSC") && getCourseNumber(c) >= 151) || ["MAT208", "MAT218"].includes(c),
      );
      const credits = used.reduce((t, c) => t + (courseMap[c]?.credits || 0), 0);
      return { completed: credits >= (block.minCredits || 32), completedCourses: used, completedCredits: credits, detail: `${credits} / ${block.minCredits || 32} cr` };
    }

    if (block.ruleType === "must_take") return { completed: block.courseCodes.length > 0 && completedCourses.length === block.courseCodes.length, completedCourses, completedCredits };
    if (block.ruleType === "choose_one") return { completed: completedCourses.length >= 1, completedCourses, completedCredits };
    if (block.ruleType === "choose_n") return { completed: completedCourses.length >= (block.minCount || 1), completedCourses, completedCredits };
    if (block.ruleType === "choose_credits") return { completed: completedCredits >= (block.minCredits || 0), completedCourses, completedCredits };
    return { completed: false, completedCourses, completedCredits };
  }

```

What functionality does your code sample abstract away?

getBlockStatus() abstracts away all the logic for determining whether a major requirement block has been satisfied. Callers never need to know what rule type a block is (must_take, choose_one, choose_n, choose_credits, or special cases like ANTH_FOUR_FIELDS). They simply pass in a block object and receive back a uniform { completed, completedCourses, completedCredits, detail } result.

How does it mechanically achieve abstraction?

The function acts as a single entry point that handles all the different types of requirement blocks internally. Depending on what kind of block it receives, it takes a completely different path to figure out whether that block is complete, but no matter which path it takes, it always hands back the same type of answer: a simple object that says whether the block is done, which courses counted toward it, how many credits those courses add up to, and a short human readable detail string.

For example, when the function encounters the Anthropology four fields block, it cannot simply count how many courses from a list the student has taken. Instead, it has to look at every Anthropology course the student planned, check what academic subfields each of those courses belongs to (things like cultural anthropology, archaeology, or biological anthropology), collect all the unique subfields it finds, and then decide the block is complete only if the student has covered at least three distinct subfields. That is a fundamentally different kind of check because it is about breadth across categories, not just quantity. Meanwhile, a standard must take block just checks whether specific required course codes appear in the student's plan, and a choose N block checks whether enough courses from an approved list are planned. Each of these needs its own logic.

The key design decision is that all of this complexity is buried inside the function and never leaks out. The part of the code responsible for drawing the requirement tree on screen never has to ask whether a block is a four fields block or a choose N block. It just asks the function whether the block is done and gets back a clean, consistent answer it can work with. This means the visual display code stays simple and readable, and all the complicated business rules about what counts as done for each block type live in one place where they can be understood and changed without touching the display at all.

What purpose does this abstraction serve in your larger program?

The abstraction enforces a clean boundary between evaluation logic and display logic. Adding a new rule type only requires adding a branch inside the status checking function, and none of the visual components that render the tree or calculate the progress bar percentage need to change at all. It also makes the requirements component easy to reuse across different majors. Swapping from Computer Science to Anthropology to Economics only changes the data being passed into the component, not any of the evaluation code itself.