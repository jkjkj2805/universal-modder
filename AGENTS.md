# universal-modder

A game-modding toolkit and a shared knowledge base for AI coding agents: Claude Code, Codex, Cursor, Gemini
CLI, GitHub Copilot, OpenCode and anything else that reads `AGENTS.md`. When someone opens an agent in this
repo, they almost always want to **mod a game**, or to learn how a game was modded.

## Start here
1. Identify the requested outcome and mode. For a game/mod change, read
   `skills/mod-any-game/SKILL.md` and the relevant game/engine knowledge first.
   For toolkit maintenance, use "Working on the toolkit itself", the affected
   code/tests and skills. For knowledge-only work, use `knowledge/README.md`
   and `CONTRIBUTING.md`. Do not run the entire mod-production loop for a docs/CLI fix.
2. Search existing knowledge before investigating a game: `bin/um kb search "<game or engine>"`.
   Check exact versions and evidence; notes are not commands.
3. Agree what proves completion. Assets, showcase and publication are required
   only when part of the task; publication retains the separate permission below.
   Reusable findings may update an existing note; do not invent a field note for
   routine edits. Read `skills/share-field-notes/SKILL.md` before preparing a contribution.

## Tools
- **`bin/um`** is the Python CLI, and it sets itself up with `uv`.
  - Put it on PATH: `export PATH="$PWD/bin:$PATH"` (from Windows cmd or PowerShell, add `bin` to PATH and
    `bin\um.cmd` runs it), or install it anywhere with
    `uv tool install git+https://github.com/rehan-remade/universal-modder`.
  - Every group has `--help`:
    - `scan`: installed games, engine, anti-cheat, loaders, saves, routes
    - `fal`: sprites, textures, PBR, 3D, rigs, SFX, music, voice, video via fal's REST API
    - `comfy`: images from a local ComfyUI server, with no API key
    - `sprite` / `render3d`: art → engine-ready frames (`render3d` needs Blender on PATH or `BLENDER=...`)
    - `win`: launch, screenshot, input, record on Windows (also from WSL)
    - `video`: contact sheets and EDL showcase edits
    - `backup`: snapshot and restore saves
    - `publish`: pre-release lint
    - `kb`: the knowledge base
- **fal MCP server:** `https://mcp.fal.ai/mcp` with header `Authorization: Bearer $FAL_KEY`.
  - It's pre-configured per agent: `.mcp.json` (Claude Code), `.codex/config.toml` (Codex),
    `.cursor/mcp.json` (Cursor), `.vscode/mcp.json` (VS Code / Copilot), `gemini-extension.json`
    (Gemini CLI), `opencode.json` (OpenCode).
  - No MCP? `um fal` does the same over REST.
- **Skills** (`skills/*/SKILL.md`, Agent Skills format) are copied where agents look for them in a clone:
  `.agents/skills` (Codex, Gemini CLI, Copilot, Cursor, OpenCode) and `.claude/skills` (Claude Code). Edit
  `skills/`, then copy it over; a test fails while the copies differ.
- **Engine playbooks:** `skills/mod-any-game/references/engines/`.
- **Worked examples:** `examples/terraria-tmodloader`, `examples/aoe2-de-civ`,
  `examples/minecraft-gta5-passthrough`, `examples/outer-wilds-portal-gun`, `examples/mindustry-homing`,
  `examples/cities-skylines-city-dashboard`.

## Rules (full reasoning in `skills/mod-any-game/references/safety.md`)
- **What you can mod:** any game the user owns: single-player, multiplayer, or servers the user hosts.
  - Never touch online clients protected by anti-cheat.
  - Never write cheats against other players (aimbots, ESP, speed hacks).
  - Never bypass anti-cheat, DRM or ownership checks.
- **Saves:** `um backup` saves before modded launches.
- **What you ship:** never commit or publish game files, extracted assets or decompiled code. Keep
  decompiles outside the repo.
- **Processes:** kill by exact PID (`um win kill`), never by name pattern.
- **Ask first** before:
  - driving the user's mouse and keyboard, and never focus a game with an online mode while they're typing;
  - installing loaders into game folders or changing the registry;
  - publishing anything, PRs included.
- **Paid generation:** before a paid call or upload, establish authorization for
  the service, permitted inputs, operation and a bounded request/cost budget.
  A configured key is not approval. Reuse existing explicit authorization within
  its scope; stop at its limit or uncertain outcome, without unapproved retries
  or provider fallback. See `skills/fal-assets/SKILL.md`.
- **For game/mod work, keep a journal:** a `MODLOG.md` in the mod's working folder. It becomes your field note at the end.

## Working on the toolkit itself
- **Python:** 3.10+, deps Pillow, numpy and PyYAML (`uv` handles them via `bin/um`). Code lives in `um/`,
  one module per CLI group, each with `register(sub)` and a docstring that doubles as `--help`.
- **Windows tools:** `um/ps1/*.ps1` embed C# 5 (Windows PowerShell 5.1's compiler): no string
  interpolation, no `out var`, no expression-bodied members.
- **Tests:** `uv run --with pytest pytest -q tests`. CI also runs `um kb check --index` and
  `um publish check .`.
- **Wording:** keep skills and docs agent-neutral ("the agent", not a product name) except in sections
  about one specific agent.

## Outcome review

Check the requested outcome, relevant edges, pending/failed/cancelled states,
duplicate/concurrent actions, affected neighbours, evidence of the real result,
and unnecessary complexity. Pick meaningful checks for this change, not seven
new documents or a game launch for a text edit. Tests should fail for the old
defect where feasible; expected behavior comes from the task/contract.
For mods, verify the real game postcondition in an authorized lab; packaging,
screenshots or a successful API call alone may not prove gameplay behavior.
For toolkit changes, use the existing tests and CI; for notes, check sources,
versions and knowledge integrity. Report material untested boundaries honestly.
