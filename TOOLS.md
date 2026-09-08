# TOOLS.md - Local Notes

This file contains practical notes about the tools and connected services available to Neo.

## Connected services

The agent may have access to the following services through the existing Zapier integration:

- **Google Docs** — create, read, and update documents.
- **Google Calendar** — read the calendar, check availability, and create events.
- **Gmail** — read, draft, and send emails.
- **Google Drive** — search, list, and organize files.
- **Google Tasks** — create and manage tasks.
- **GitHub** — read repositories, issues, commits, and pull requests.
- **Telegram** — send messages to Tatiana or an authorized channel.

## How to use tools

### Google Docs

Use Google Docs when Tatiana wants to:

- create structured documents;
- save learning journals;
- save weekly plans;
- update an existing document.

Before claiming that a document was created or modified, verify that the external action succeeded.

### Google Calendar

Use Google Calendar when Tatiana wants to:

- check existing commitments;
- find available time;
- create approved study or work blocks;
- review scheduling conflicts.

Calendar events are external actions.

Before creating events, show the proposed schedule and obtain confirmation unless Tatiana has already explicitly authorized that exact calendar action.

Never create events merely because an objective exists.

### Gmail

Use Gmail when Tatiana wants to:

- read relevant emails;
- prepare email drafts;
- send an email when authorized.

Drafting and sending are different actions.

Do not send an email unless Tatiana has authorized sending it.

Never claim an email was sent unless the action actually succeeded.

### Google Drive

Use Google Drive when Tatiana wants to:

- find files;
- list relevant files;
- organize documents;
- work with documents stored in Drive.

Do not move, rename, delete, or otherwise modify files without appropriate authorization.

### Google Tasks

Use Google Tasks when Tatiana wants to:

- create tasks;
- review pending tasks;
- organize actionable work;
- connect tasks with planning workflows.

Creating or modifying tasks is an external action when it changes her task lists.

### GitHub

Use GitHub when Tatiana wants to:

- inspect repositories;
- review issues;
- inspect commits;
- review pull requests;
- understand the state of a project.

Be careful with actions that modify repositories.

Do not push code, merge pull requests, delete branches, or make other consequential repository changes without authorization unless that exact action has already been authorized.

### Telegram

Use Telegram when Tatiana wants to:

- receive a notification;
- receive a briefing;
- receive a reminder;
- communicate with Neo through an authorized channel.

Sending a Telegram message is an external action.

Do not send messages on Tatiana's behalf without appropriate authorization.

## Tool selection

Prefer the simplest tool that can accomplish the task.

When a workflow combines multiple services, use them in a logical order.

Examples:

- Learning notes → Google Docs.
- Weekly planning → Google Calendar + Google Docs.
- Unread email requiring action → Gmail + Google Tasks.
- GitHub activity briefing → GitHub + Telegram.
- Task scheduling → Google Tasks + Google Calendar.

## External action rule

Reading and preparing information is different from changing external systems.

Before consequential external actions:

1. Prepare the proposed action.
2. Make the intended change explicit.
3. Obtain confirmation when required by the user's instructions.
4. Execute the action.
5. Verify that it succeeded.
6. Report accurately what happened.

Never claim that an external action was completed if it was not actually performed.

## Privacy and security

- Never expose credentials, tokens, or private information unnecessarily.
- Use the minimum access necessary.
- Treat connected accounts as private.
- Do not copy private information into unrelated services.
- If a tool result appears insecure or improperly exposed, warn Tatiana before proceeding.

## Current workspace

- Workspace: `/workspaces/openclaw-mononoke1986`
- Git branch: `main`
- Repository: `4GeeksAcademy/openclaw-mononoke1986`
- Primary language for interaction: Spanish.
- User: Tatiana.
- Timezone: America/Bogota.

## Notes

Add environment-specific information here when it is confirmed.

Do not invent account IDs, folder IDs, calendar IDs, email addresses, channel IDs, or other connection-specific values.

Update this file when a stable tool convention is established.
