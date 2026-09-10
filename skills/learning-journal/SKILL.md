---
name: learning-journal
description: Transforma notas informales de aprendizaje en una entrada estructurada de diario de aprendizaje.
---

# Learning Journal

## Purpose

Transform Tatiana's informal learning notes into a clear, structured learning-journal entry without inventing information.

## When to use

Use this skill when Tatiana wants to:

- record what she learned;
- organize notes from a study session;
- reflect on what she understood;
- identify concepts that remain unclear;
- document exercises or practical work;
- decide what to review next.

## Input

Tatiana may provide her notes in natural language.

The notes do not need a predefined format.

Use the workspace context to understand that Tatiana is learning programming, JavaScript, TypeScript, Python, APIs, Git, web development, and AI engineering.

## Processing rules

1. Preserve the meaning of Tatiana's original notes.
2. Do not invent facts, results, understanding, or conclusions that are not present.
3. Separate clearly:
   - what was learned;
   - what was understood;
   - what remains unclear;
   - what was practiced;
   - what should be reviewed.
4. If something is uncertain, preserve the uncertainty.
5. Prefer concise explanations over unnecessary repetition.
6. When useful, explain the reasoning behind a concept rather than merely naming it.
7. Identify practical examples or exercises mentioned in the notes.
8. Suggest a concrete next step based only on the available context.

## Output format

Produce the following structure:

# Learning Journal — YYYY-MM-DD

## Topic

The main subject of the learning session.

## What I learned

A concise summary of the new knowledge.

## Key concepts

- Concept 1
- Concept 2
- Concept 3

## What I understood well

What appears clear from the notes.

## What is still unclear

Questions, gaps, mistakes, or concepts that need review.

## Practical examples or exercises

Relevant exercises, commands, code, examples, or applications mentioned by Tatiana.

## Questions to revisit

Specific questions that should be answered later.

## Suggested next step

One or more concrete actions for the next study session.

## Quality criteria

A successful result:

- accurately represents Tatiana's notes;
- does not manufacture certainty;
- distinguishes understanding from uncertainty;
- preserves useful technical details;
- provides a practical next step.

## Storage

The resulting entry should be prepared for storage in the designated Google Docs learning journal when the required Google Docs integration is available.

Do not claim that the entry was saved to Google Docs unless the external action actually succeeded.

## External actions

Creating or modifying a Google Doc is an external action.

If the required Google Docs tool is available, prepare the content first and make the external write only when Tatiana has authorized that action or the workflow explicitly authorizes it.

Never claim an external document was created or modified if the action was not actually performed.
