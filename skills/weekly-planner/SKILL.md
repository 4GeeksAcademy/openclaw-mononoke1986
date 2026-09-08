---
name: weekly-planner
description: Convierte objetivos, compromisos y tiempo disponible en un plan semanal priorizado y realista.
---

# Weekly Planner

## Purpose

Turn Tatiana's weekly objectives, commitments, deadlines, and available time into a prioritized and realistic weekly plan.

When authorized, the skill can prepare approved study or work blocks for Google Calendar.

## When to use

Use this skill when Tatiana wants to:

- organize her week;
- prioritize study or work objectives;
- plan around deadlines;
- identify conflicts;
- distribute learning tasks realistically;
- prepare calendar blocks.

## Input

Tatiana may provide her objectives, commitments, deadlines, and constraints in natural language.

The agent may use available workspace context and connected services such as:

- USER.md;
- Google Calendar;
- Google Tasks;
- relevant workspace context.

## Planning rules

1. Identify fixed commitments first.
2. Identify deadlines and time-sensitive objectives.
3. Separate:
   - fixed commitments;
   - deadlines;
   - high-priority objectives;
   - flexible tasks;
   - optional activities.
4. Do not assume every objective deserves calendar time.
5. Prioritize based on urgency, importance, dependencies, and available time.
6. Avoid unrealistic workloads.
7. Detect scheduling conflicts.
8. Preserve recovery and unallocated time when appropriate.
9. If an important constraint is missing but a reasonable assumption is possible, state the assumption and continue with a provisional plan.
10. Do not create calendar events merely because an objective exists.

## Output format

Produce:

# Weekly Plan — YYYY-MM-DD

## Priorities

Ordered list of the most important objectives for the week.

## Fixed commitments

Existing commitments that constrain the schedule.

## Deadlines

Important dates and deliverables.

## Planned work

For each planned activity:

- Objective
- Priority
- Estimated duration
- Proposed day
- Reason for placement

## Calendar blocks

For each proposed block:

- Date
- Start time
- End time
- Title
- Purpose

## Conflicts and risks

Identify:

- overlapping commitments;
- unrealistic workloads;
- insufficient time;
- missing information;
- dependencies that could affect the plan.

## Assumptions

Explicitly list assumptions used to create the provisional plan.

## Quality criteria

A successful result:

- reflects existing commitments;
- makes priorities explicit;
- fits the available time;
- avoids unrealistic scheduling;
- identifies meaningful conflicts and risks;
- clearly distinguishes proposed actions from completed actions.

## Google Calendar

Creating calendar events is an external action.

Before creating events, present the proposed calendar blocks clearly and obtain confirmation unless Tatiana has already authorized that exact calendar action.

Only create the approved blocks.

Never claim that an event was created unless the calendar action actually succeeded.

## Google Docs

The finalized weekly plan should be prepared for storage as a new Google Doc when the required Google Docs integration is available.

Never claim that a Google Doc was created unless the external action actually succeeded.
