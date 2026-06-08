---
name: work-sync
description: Synchronize a Dashboard Hub project without starting a work session. Use when the operator says "/동기화", "슬래쉬 동기화", "작업 동기화", "동기화", "/sync", or wants to pull the latest project state on this device without marking the project as 작업중. Applies to every project in Dashboard-hub.
---

# 작업 동기화

Run the common Dashboard Hub sync workflow.

## Workflow

1. Locate `Dashboard-hub`:
   - Prefer `C:\Users\동민\main\dashboard-hub`.
   - If missing, try `C:\Users\203470\main\dashboard-hub`.
   - Otherwise search upward from the current workspace for `dashboard-hub`.
2. Read `dashboards.json`.
3. Resolve `ProjectId` from the user's wording or current directory:
   - Match `id`, `name`, `label`, or `projectPath`.
   - If current directory is inside a registered `projectPath`, use that project.
   - If missing, do not ask by default. Run common sync with no `ProjectId`; it syncs every project that the cloud Hub marks as sync-needed for the current device.
   - If ambiguous because the user named multiple possible projects, ask one short question.
4. Run one of:
   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File <hub>\sync-work.ps1 -ProjectId <project-id>
   ```
   or, when no single project was specified:
   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File <hub>\sync-work.ps1
   ```
   In no-project mode, this syncs all sync-needed projects in parallel.
   Add `-Note "<note>"` only when the user provided a note.
   If `DONGPA_HUB_URL` is configured, the script records to the cloud Hub. Otherwise it uses local `127.0.0.1`.
5. Do not mark the project as 작업중. Do not commit, push, deploy, restart services, or publish static dashboards.
6. Do not bypass device restrictions. If `lab-status` is blocked on PC, report that it is notebook-only and sync is unnecessary there.
7. Report concise Korean output:
   - project synced
   - device
   - whether Git pull ran
   - any block reason or local dirty state

## Project Rule

This skill is common for all Dashboard Hub projects. Project-specific behavior must come from `dashboards.json` and `work-common.ps1`, not hardcoded skill logic.
