# SKILLS DESIGN

## Skill 1: Learning Journal

### 1. What does this skill do?

Transforms Tatiana's daily learning notes into a structured learning journal entry and saves it to a Google Doc.

### 2. What input does the agent need?

Tatiana provides informal notes about what she learned, understood, practiced, or found difficult.

The input can be written in natural language and does not require a fixed format.

The agent already knows from the workspace configuration that:
- Tatiana is learning programming, JavaScript, TypeScript, Python, APIs, Git, web development, and AI engineering.
- She prefers direct, practical explanations.
- She wants to understand the reasoning behind solutions.
- She benefits from separating facts, assumptions, interpretations, and predictions.

The agent should infer the structure from the notes rather than requiring Tatiana to format them manually.

### 3. What is a good output?

The skill produces a structured learning-journal entry containing:

- Date
- Topic
- What I learned
- Key concepts
- What I understood well
- What is still unclear
- Practical examples or exercises
- Questions to revisit
- Suggested next step

The entry is added to the designated Google Doc learning journal.

A successful execution means:
1. The content is structured without losing the meaning of the original notes.
2. Uncertainty is preserved rather than invented away.
3. The journal entry is saved successfully in Google Docs.
4. The agent reports what was saved and where.

---

## Skill 2: Weekly Planner

### 1. What does this skill do?

Turns Tatiana's weekly objectives, commitments, and available time into a prioritized weekly plan and, after confirmation, creates the relevant study or work blocks in Google Calendar.

### 2. What input does the agent need?

Tatiana provides her objectives, commitments, deadlines, and relevant constraints for the week in natural language.

The agent can use information already available from:
- USER.md
- Google Calendar
- Google Tasks
- Existing workspace context

The agent should distinguish between:
- Fixed commitments
- Deadlines
- High-priority objectives
- Flexible tasks
- Optional activities

The agent should not assume that every objective deserves calendar time.

If an important constraint is missing but a reasonable assumption is possible, the agent should state the assumption and proceed with a provisional plan.

Before creating external calendar events, the agent must make the proposed schedule explicit and obtain confirmation unless Tatiana has already authorized that exact calendar action.

### 3. What is a good output?

The skill produces:

1. A prioritized weekly plan.
2. A short explanation of the prioritization.
3. A list of proposed calendar blocks with:
   - Date
   - Start time
   - End time
   - Title
   - Purpose
4. Any detected conflicts, unrealistic workloads, or missing constraints.

The weekly plan is saved as a new Google Doc.

After Tatiana confirms the proposed calendar blocks, the approved blocks are created in Google Calendar.

A successful execution means:
1. The plan reflects existing calendar commitments.
2. Priorities are explicit.
3. The workload is realistic.
4. Calendar events are not created without the required confirmation.
5. The final document and calendar actions are reported clearly.
