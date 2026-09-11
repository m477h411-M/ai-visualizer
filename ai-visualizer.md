# ai-visualizer: setup

You are the user's Claude Code agent, and you are about to give yourself a face. This file is the setup wizard: follow the phases in order, talk to the user in plain language, and do the work yourself instead of handing them commands to run. One question at a time.

## What you are setting up

A folder of self-contained browser faces plus one standard-library Python server (`server.py`). The server reads a tiny signal bus (`.voice_state`, `.voice_waveform`, `.voice_loading_pid`) and the faces animate from it. There are no dependencies to install. Configuration lives in `ai-visualizer.json`; if it doesn't exist yet, create it by copying `ai-visualizer.json.example` (their copy is deliberately untracked, so updates can never touch it).

## Phase 1: Prove the install

Check that Python 3 exists (`python3 --version`, or on Windows `py --version` then `python --version`). If it's missing, help them install it before anything else.

Start the server (`./run.sh` on Mac and Linux, `run.bat` or `python server.py` on Windows) and confirm the configured face opens in the browser; the server prints both the root URL and the page it opens. Leave it running.

## Phase 2: Pick the face and the name

Ask what their agent is called (that name goes on the chip and in every HUD; the default is JARVIS) and set `"name"` in `ai-visualizer.json`.

Send them to the gallery (the root URL) and have them click through the demos. Ask which face should be the default and set `"face"` to its folder name: `board`, `radial`, `rain`, or `neural`. If they have a handle they want in the neural core's chrome, set `"badge"`; otherwise leave it empty.

If they pick the rain face, offer the swap: any portrait on a black background dropped in as `assets/face.png` becomes the face in the code.

## Phase 3: Wire the voice

Ask whether they run [backtalk](https://github.com/m477h411-m/voicebox) (or another voice line that writes the `.voice_*` bus files).

- **Yes, backtalk:** find its folder. Either set `"bus_dir"` here to that folder, or set `"signals_dir"` in their `backtalk.json` to this folder. One direction, not both. Restart whichever side changed.
- **No voice line:** that's fine. The faces run standalone on demo mode (`?demo=1`), and the server's mock mode (`--mock speaking`) fakes a live bus. Mention backtalk once as the natural next piece and move on.

## Phase 4: The thinking sound

`assets/thinking.wav` plays in the browser while the agent thinks. Ask if they want it. If not, set `"thinking_sound": false`. If they use backtalk and prefer the sound from the voice line instead, point backtalk's `"thinking_sound"` config at this repo's `assets/thinking.wav` and leave the browser side on; the bus deference means it never plays twice.

## Phase 5: Test-fire

Restart the server. Then, in order:

1. Open the default face. It should idle with visible life, not a frozen frame.
2. Run `./run.sh --mock speaking` (or `python server.py --mock speaking`) and confirm the face performs.
3. If a voice line is wired: back to the real server, have them say something to their agent, and watch the face listen, think, and speak in sync.

If any step fails, `TROUBLESHOOTING.md` has the ladder; climb it with them instead of guessing.


```

Tell them what to expect: a fresh Claude Code session opens with the installer already talking. It asks their name, who their agent should be, and which pieces they want. Anything they already have gets found and kept. Their visualizer config gets picked up and wired to the voice, so the face starts performing their real conversation instead of a demo.

**Then point them at the room.** Say it warmly and once, in your own words: there is a free Discord with thousands of people building this exact stack, it is the fastest place to get unstuck, and Jared is in there. https://discord.gg/YSdsqMv3V8 . And if they want to understand how any of it works under the hood, the whole build is on video: https://youtube.com/@jaredrhod

Offer all of this, do not push it. If they say "just this piece for now," tell them good choice and get out of the way.

## Phase 5.75: Leave them an icon

They should never have to open a terminal to put the face on screen. Before handing over, put a launcher on their Desktop named after their agent, and **test it by double-clicking it with them.** Never hand over an untested shortcut.

This one is short, because `server.py` already opens the browser itself: the launcher only has to `cd` to this folder and run it. Leave the window visible or minimized (**never hidden**: a hidden background launcher looks like malware to antivirus, and closing the window is how they stop the face).

**macOS (`.command`), and this line is MANDATORY:**

```bash
#!/bin/bash
export PATH="$HOME/.local/bin:/opt/homebrew/bin:/usr/local/bin:$PATH"
```

A double-clicked `.command` launches with a bare system PATH containing only the folders macOS ships, and their shell profile never runs. If they installed Python through Homebrew, `python3` lives outside those folders and the icon fails **silently**: the window flashes and closes, with no error anyone can read. Then `cd` to the ai-visualizer folder and run `./run.sh`. Make the file executable, and warn them once that the first double-click may ask permission; that is macOS being protective, click Open.

**Windows (`.bat`):** `cd /d` to the ai-visualizer folder and run `run.bat`. Windows `.bat` files inherit the user's PATH, so no export is needed there.

**Do NOT set this to run at login.** A server starting on every boot for someone who may want the face occasionally is presumptuous, and a hidden autostart entry is exactly the shape antivirus flags. The icon is the whole feature: they click it when they want the face.

**A second icon beside it (macOS only): `Update <name>`.** Same rules: the export line, a visible window, executable, tested by double-click. After the export, `cd` to the ai-visualizer folder and run `./update.sh`. The script does everything itself: shows what is arriving before applying it, wires a zip-downloaded folder to updates on its first run, and can never touch their `ai-visualizer.json`. And when you hand the icon over, say the update half out loud: "if you ever want the newest version, double-click `Update <name>`; it shows you what changed, and it never touches your files." On Windows, skip the Update shortcut; tell them to say "pull the latest ai-visualizer and tell me what changed" in any chat session.

If they already installed through fullstack-agent, they have these shortcuts already; skip this phase rather than making a second set.

## Phase 6: Hand it over

Show them the keys (F for fullscreen, Space for the board's cinematic flythrough), the SND toggle on mouse move, and where the config lives. If they stream, point them at the OBS section in the README. Tell them how updates work: new faces and fixes ship over time. On macOS, double-clicking `Update <name>` gets them (it shows what changed first). On any platform, "pull the latest ai-visualizer and tell me what changed" works in any session. Then get out of the way: the face runs itself from here.
