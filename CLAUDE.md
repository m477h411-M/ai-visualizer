# Repo Boot — ai-visualizer

<!-- This is the REPOSITORY's own boot file, loaded by Claude Code when `claude`
     runs inside a clone of this repo. It is not part of the shipped program, and
     it is NOT the person's agent config — their agent lives wherever their own
     CLAUDE.md is, and the visualizer only puts a face on it. -->

You are running inside a clone of **ai-visualizer**.

Unless the person says otherwise, assume they are here to get the face on screen.

**Do this first.** Read `ai-visualizer.md` at the root of this repo and execute it. It is a setup wizard, written to be followed phase by phase with the person — not summarized, not described, not skimmed for highlights. Start at Phase 1 and work through in order, one question at a time, doing the work yourself rather than handing them commands to run.

Open with one line, then wait for a yes. Something like: "I've got the ai-visualizer setup wizard loaded — want me to give you a face? There's nothing to install, so this is quick."

If they say they are only looking around, or ask a question about the code, answer it normally and leave the wizard alone until they ask for it.

## Things to get right

- **There is nothing to install.** Python 3's standard library and a browser, that is the whole dependency list. Do not create a virtualenv, do not `pip install` anything, do not add a package to make something easier.
- **The server runs from this folder.** It binds to 127.0.0.1 only and serves nothing outside this directory. `ai-visualizer.json` lives here too. Leave the server running during setup, and restart it after a config change before testing.
- **`ai-visualizer.json` is theirs and untracked.** Create it from `ai-visualizer.json.example` if it's missing. Never overwrite an existing one without asking.
- **Wire the signal bus in ONE direction, never both.** Either `bus_dir` here points at their voice-line folder, or `signals_dir` in that voice line's config points at this folder. Setting both is how the two ends end up disagreeing about where the bus lives. Restart whichever side you changed.
- **No voice line is a fine answer.** The faces run standalone on demo mode (`?demo=1`), and `--mock speaking` fakes a live bus. Mention a voice line once as the natural next piece and move on.
- When a test-fire step fails, `TROUBLESHOOTING.md` has the ladder. Climb it with them instead of guessing.

## What's in here

- `ai-visualizer.md` — the setup wizard. Phases 1 through 6.
- `faces/` — the four shipped faces (`board`, `radial`, `rain`, `neural`), each a folder with its own `index.html`. A new folder dropped in here appears in the gallery automatically.
- `core.js` — the shared bus client every face calls: `AV.init()`, then `AV.state`, `AV.env` and `AV.samples` in the draw loop.
- `server.py`, `run.sh`, `run.bat` — the server; `--mock speaking` rides a synthetic bus.
- `index.html` — the gallery at the root URL, with one-click demos of all four.
- `skills/ai-visualizer-setup/` + `.claude-plugin/` — the same setup, packaged so any Claude Code session can bootstrap an install.
