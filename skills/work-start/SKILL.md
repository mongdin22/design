---
name: work-start
description: Start a Dashboard Hub work session for any registered project. Use when the operator says "/시작", "슬래쉬 시작", "작업 시작", "시작", "/start", or asks to begin working on a project/device workflow. Applies to every project in Dashboard-hub, not only one project.
---

# 작업 시작

Run the common Dashboard Hub start workflow.

Universal rule:

- Use `/시작` for every registered project when work begins.
- Device-scoped projects still use `/시작`; the scope controls where work may begin.
- For notebook-only projects such as `lab-status`, start on the notebook. Do not start from PC just to mark status.

## Workflow

1. Locate `Dashboard-hub`:
   - Prefer `C:\Users\동민\main\dashboard-hub`.
   - If missing, try `C:\Users\203470\main\dashboard-hub`.
   - Otherwise search upward from the current workspace for `dashboard-hub`.
2. Read `dashboards.json`.
3. Resolve `ProjectId` from the user's wording or current directory:
   - Match `id`, `name`, `label`, or `projectPath`.
   - If current directory is inside a registered `projectPath`, use that project.
   - If ambiguous or missing, ask one short question asking which project to start.
4. Run:
   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File <hub>\start-work.ps1 -ProjectId <project-id>
   ```
   Add `-Note "<note>"` only when the user provided a note.
   If `DONGPA_HUB_URL` is configured, the script records to the cloud Hub. Otherwise it uses local `127.0.0.1`.
5. If the resolved project is `crypto-gap`, merge the old Crypto Gap start workflow into this same `/시작` run:
   - First run `start-work.ps1` as above so Hub shows `작업중`.
   - Then read and follow `C:\Users\동민\.codex\skills\crypto-gap-start\SKILL.md`.
   - Treat Crypto Gap repository runbook as authoritative for pull, cloud state, automation, and manual-action checks.
   - Do not ask the operator to run a separate Crypto Gap skill.
6. Do not bypass device restrictions. If `lab-status` is blocked on PC, report that editing/starting is notebook-only and that PC should use the cloud URL to view the published dashboard.
7. Do not deploy from generic start. Crypto Gap cloud checks may run, but deploy/restart stays governed by the Crypto Gap runbook.
8. Report concise Korean output:
   - project started
   - device
   - Git dirty / pull needed if reported
   - any block reason
   - for `crypto-gap`, include cloud automation/manual-action summary from the delegated runbook

## Project Rule

This skill is common for all Dashboard Hub projects. Project-specific behavior must come from `dashboards.json` and `work-common.ps1`, not hardcoded skill logic.
