---
name: work-end
description: End a Dashboard Hub work session for any registered project. Use when the operator says "/끝", "슬래쉬 끝", "작업 끝", "끝", "/end", or asks to finish, stop, pause, move devices, or close a project work session. Applies to every project in Dashboard-hub, not only one project.
---

# 작업 끝

Run the common Dashboard Hub end workflow.

Universal rule:

- Use `/끝` for every registered project when work ends.
- For `cloud-static` projects, `/끝` is both work-session close and static dashboard publish.
- For notebook-only projects such as `lab-status`, run `/끝` on the notebook after local edits. This publishes the completed dashboard so PC can view the latest cloud copy.
- Do not run `/끝` from PC to bypass notebook-only work restrictions.

## Workflow

1. Locate `Dashboard-hub`:
   - Prefer `C:\Users\동민\main\dashboard-hub`.
   - If missing, try `C:\Users\203470\main\dashboard-hub`.
   - Otherwise search upward from the current workspace for `dashboard-hub`.
2. Read `dashboards.json`.
3. Resolve `ProjectId` from the user's wording or current directory:
   - Match `id`, `name`, `label`, or `projectPath`.
   - If current directory is inside a registered `projectPath`, use that project.
   - If missing, do not ask by default. End every cloud-Hub active project for the current device, including delegated projects.
   - If ambiguous because the user named multiple possible projects, ask one short question.
4. If a single resolved project is `crypto-gap`, merge the old Crypto Gap finish workflow into this same `/끝` run:
   - Read and follow `C:\Users\동민\.codex\skills\crypto-gap-finish\SKILL.md`.
   - Treat Crypto Gap repository runbook as authoritative for validation, safe commit/push, deploy, and cloud verification.
   - Do not ask the operator to run a separate Crypto Gap skill.
   - After the Crypto Gap finish workflow completes or safely stops, continue with `end-work.ps1` below so Hub records the final state.
5. If no single project was specified:
   - Read cloud Hub sessions from `<hub-api>/api/sessions`.
   - Determine current device with `DONGPA_DEVICE`/`COMPUTERNAME`; compare device scopes, not exact names (`notebook`, `laptop`, `노트북` are the same scope).
   - For each active delegated project on this device, run its delegated finish first. Currently this means `crypto-gap` -> `crypto-gap-finish`.
   - After each delegated finish completes or safely stops, run:
     ```powershell
     powershell -NoProfile -ExecutionPolicy Bypass -File <hub>\end-work.ps1 -ProjectId <delegated-project-id>
     ```
     This records the Hub session end.
   - Then run:
     ```powershell
     powershell -NoProfile -ExecutionPolicy Bypass -File <hub>\end-work.ps1
     ```
     This closes all remaining non-delegated active projects for the current device in parallel. Crypto Gap remains sequential because deploy and cloud verification must stay isolated.
6. If a single non-delegated project was specified, run:
   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File <hub>\end-work.ps1 -ProjectId <project-id>
   ```
   Add `-Note "<note>"` only when the user provided a note.
   If `DONGPA_HUB_URL` is configured, the script records to the cloud Hub. Otherwise it uses local `127.0.0.1`.
7. For projects with `publishMode = cloud-static` and `workflows.end.publish = true`, this end script publishes the completed static dashboard to the cloud Hub via `publish-static.ps1`. This is publish, not app/server deploy.
8. For `lab-status`, edit locally in `C:\Users\동민\main\역학`, then end from the notebook. The end flow publishes the completed `00_랩미팅_향후계획` dashboard package to `http://100.84.105.126:8844/dashboard-pages/lab-status/current/dashboard.html`, which is what PC should open.
9. Do not bypass device restrictions.
10. Generic end does not deploy app servers. Crypto Gap deploy/restart is allowed only through the delegated Crypto Gap finish workflow and its active-execution safety gate.
11. Report concise Korean output:
   - project ended
   - device
   - Git dirty / push needed if reported
   - cloud-static publish result when publish ran
   - whether it is safe to move device or rest
   - for `crypto-gap`, include commit/push/deploy/cloud verification result from the delegated runbook

## Project Rule

This skill is common for all Dashboard Hub projects. Project-specific behavior must come from `dashboards.json` and `work-common.ps1`, not hardcoded skill logic.

If Git remains dirty or needs push, do not claim the project is fully transferable. Say the end record was written but commit/push is still needed before switching devices.
