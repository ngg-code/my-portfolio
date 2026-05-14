+++
date = '2026-05-11T14:54:22-05:00'
draft = false
title = 'UI Design'
+++

src/components/major_requirements.jsx lines 213–354 (tree view) and src/components/semestersTable.jsx lines 64–113 (inline-edit slots)

Visibility of system status : 

Status dots change from gray → green/red only after "Check Requirements" is clicked, giving explicit feedback about what the system knows vs. what it hasn't evaluated. The progress bar shows the correct percentage (line 191: transition: "width 0.4s ease"), showing that a state change just occurred.

Direct manipulation: 

Clicking a semester slot transforms it in-place into a <select> dropdown (semestersTable.jsx line 73). The slot itself is the interaction target, not a separate edit button elsewhere on the page. This follows the principle that objects should be directly manipulable.

Alternative considered and why current design prevail: 

A flat checklist (one row per requirement, all always visible) was considered, but we didn't like it because it violates the minimalist design principle — a student with 18 requirement blocks visible simultaneously while also editing their plan loses focus on the planning task. The collapsible tree (line 270) allows completed blocks to be collapsed away, keeping attention on what's incomplete. The tree also exposes the parent-child relationship between blocks and individual courses, which a flat list cannot represent.