# Delete the Apps

A single-file 3D browser game. One day into one night: start hooked on your phone, end in deep sleep.

**Play:** open `index.html` in a browser (it loads three.js and fonts from CDNs, so it needs an internet connection).

Hosted with GitHub Pages from the `main` branch.

## State machines

The game is a handful of small state machines. The first three are explicit in `index.html`; the rest are documented here so they are not rediscovered by accident.

| Machine | Lives in | States |
|---|---|---|
| Game mode | `FLOW`, `setMode` | attract, intro, play, pause, end |
| Screen (which overlay is up) | `SCREENS`, `syncScreen` | start, levels, how, card, pause, end, none |
| Cube lifecycle | `CUBE_STATES`, `ST`, `stepCube` | air, settle, idle, bump, move, convert, match, close, flip, float, deflate, sea, swallow, fall, install, dead |
| Plan countdown | `tick`, `holdFirstPlan` | travelling, waiting at the front, happened or cancelled |
| App update freeze | `tick`, `finishUpdate` | updating (`c.upd` beats left), done |
| Sunk cushion | `canStack`, `swallow` | free, sunk, stacked |
| Close budget | `closesLeft` in `close` | has closes, out |
| Hints and callouts | `hints`, `hintCheck`, `stepCallout` | each hint shown once; one callout at a time |
| Level pacing | `frame`, `script` | opening script, spawning, surge, wave, banner, algorithm, road edge |
| Progress | `progress`, `levelDone` | unlocked level, stars per level |

Rules that keep them honest:

- `setMode` warns in the console on a move `FLOW` does not list. It never blocks one.
- A delay that belongs to a mode uses `afterMode(ms, fn)`, so it is dropped if the game has left that mode.
- Use the named sets in `ST` (`LIVE`, `TICKING`, `RESTING`) instead of writing state lists inline.

Reading the game without screenshots: `<body>` carries `data-mode` and `data-screen`, and `dta.state()` returns a snapshot of every machine. `dta.check()` returns a list of broken invariants, which is empty when the game is healthy.
