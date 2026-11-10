# AGENTS.md — shared context for every AI coding agent

This file is the single source of truth for any AI agent working on Vitruvius
(Claude Code, Gemini CLI, Qwen Code, OpenCode, Aider, Cline, local models…).
Read it fully before making changes. Then read `TASKS.md` for current state.

## Product
Vitruvius is a commercial Autodesk Revit add-in. First product: repairs
reversed / garbled Hebrew text in linked DWG/DXF drawings inside Revit.
- Detects EN-typed and legacy-encoded Hebrew in linked CAD files
- Reverses and re-encodes text so it displays correctly (RTL)
- Manages SHX → TTF font mapping
- Auto-reloads the links

Status: private beta with an Israeli architecture firm. Channel: `beta`.

## Tech stack
- C# / .NET, Revit API (Revit 2024)
- AutoCAD core / DXF processing
- Windows only — Revit cannot be built or run on Linux/macOS or in cloud sessions.

## Repository layout
- `README.md` — public project overview
- `updates/manifest.json` — auto-updater channel manifest (see Release rules)
- `docs/ai-fallback/` — how to keep working with non-Claude models
- TODO(owner): add the add-in source tree here and describe it
  (solution file, main projects, entry point / IExternalApplication, tests).

## Build & test
TODO(owner): fill in the exact commands, e.g.
- Build: `dotnet build <Solution>.sln -c Release`
- Tests: `dotnet test`
- Manual check: load add-in in Revit 2024, open a sample linked DWG with Hebrew.

## Hard rules (all agents)
1. Never change Hebrew text-reversal / encoding logic without a test case that
   shows input bytes → expected output. This is the core of the product.
2. Never edit `updates/manifest.json` unless explicitly asked to cut a release.
   `downloadSha256` and `downloadBytes` must match the real installer exactly;
   a wrong value breaks auto-update for every customer.
3. Never commit secrets, API keys, customer drawings or customer names.
4. Keep changes small; one logical change per commit, clear commit message.
5. Before stopping (or when your usage is about to run out), update `TASKS.md`:
   what you did, what is half-done, the exact next step.
6. If unsure about Revit API behaviour, say so and leave a TODO — do not guess.

## Release rules
`updates/manifest.json` fields: `latestVersion`, `minimumVersion`, `channel`,
`releaseDate` (YYYY-MM-DD), `downloadUrl`, `downloadSha256`, `downloadBytes`,
`releaseNotesUrl` (GitHub release tag `vX.Y.Z`). Commit message: `release X.Y.Z`.

## Model routing guideline
- Claude: architecture, hard Revit API bugs, RTL/encoding logic, reviews.
- Free/cheap models: boilerplate, tests, docs, simple refactors, release notes.
- Any change made by a weaker model to core logic → review with Claude later.
