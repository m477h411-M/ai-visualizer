---
name: ai-visualizer-setup
description: Install and set up ai-visualizer, the animated browser face for an AI agent — a procedural circuit board, a radial particle orb, matrix rain with a face in it, or a neural core, each idling, listening, thinking and speaking in sync with a real voice line. Clones a working copy, starts the server, picks the face and the agent's name, and wires the signal bus. Use when someone asks to set up ai-visualizer, give their agent a face or avatar, put a visualizer on screen, or add visuals to their voice assistant.
---

# ai-visualizer — install and set up

ai-visualizer gives an AI agent a face: a browser page that animates from a tiny signal bus, so it idles, listens, thinks and speaks along with the real conversation. This skill gets it running on their machine.

## The one rule about where it lives

**Never run the setup against `${CLAUDE_PLUGIN_ROOT}`.** A plugin directory is replaced when the plugin updates, and the setup writes `ai-visualizer.json` into whatever folder it runs in — along with any face art they swap in and any face they add. Clone a working copy into a normal folder the person owns, and run everything there.

## Step 1 — Find or create the working copy

Ask where they want it; suggest `~/ai-visualizer` unless they have a preference. If a clone already exists there — the folder contains `server.py`, `index.html` and `ai-visualizer.json.example` — use it as-is and skip the clone.

```bash
git clone https://github.com/m477h411-M/ai-visualizer.git ~/ai-visualizer
cd ~/ai-visualizer
```

## Step 2 — Run the wizard from that folder

Read `ai-visualizer.md` at the root of the clone and **execute it**. It is a setup wizard written to be followed phase by phase with the person — not summarized, not described. Start at Phase 1 and work through in order, one question at a time, doing the work yourself rather than handing them commands.

From there the wizard owns the job: proving Python 3 is present, starting the server, picking the face and the agent's name, wiring the voice bus, the thinking sound, the test-fire, and the launcher icon.

## Worth knowing before you start

- **There is nothing to install.** Python 3's standard library and a browser. No virtualenv, no `pip install`, no added dependencies.
- **`ai-visualizer.json` is theirs and untracked.** Create it from the `.example` if missing; never overwrite an existing one without asking.
- **Wire the bus in one direction only** — either `bus_dir` here, or `signals_dir` in the voice line's config, never both — and restart whichever side changed.
- **No voice line is fine.** Demo mode (`?demo=1`) and `--mock speaking` both perform without one.
- The server binds to 127.0.0.1 and serves only its own folder. If port 8790 is taken, change it in the config.
- When a test-fire step fails, `TROUBLESHOOTING.md` in the clone has the ladder. Climb it rather than guessing.
