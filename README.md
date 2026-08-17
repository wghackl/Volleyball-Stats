# Volleyball Stats

A single-file, fully local volleyball stat tracker. No server, no install, no internet.

**To run:** double-click `index.html`. That's it.

## Where your data lives

In your browser's `localStorage` (key `vbstats.v1`) — **not** in this folder. Two things
follow from that:

- Opening `index.html` in a *different* browser or profile shows an empty app.
- Clearing your browser's site data erases your stats.

Use **Matches → Export all data (JSON)** to back up or move to another machine.

## Getting started

1. **Teams** → add your team, add players. A jersey number *or* a name is enough — neither
   is individually required.
2. **Teams** → add an opponent and its roster. For a team that won't give you names, type
   the numbers straight into the # box (`4, 9, 15, 22`) and they're added as separate
   players in one go. Rosters are reused across matches, so you build each opponent once.
3. **Matches** → pick the two teams and a date → Create.
4. **Match** → pick a player, tap stats. `+` next to the set pills starts a new set.
5. **Box Score** → full table for both teams, filterable by set, exportable to CSV.

## Phone layout

The same file reflows below 700px into a phone-first tally screen. To see it on your Mac,
just narrow the browser window (or use Safari → Develop → Enter Responsive Design Mode).

What changes:

- **Tabs move to a bottom bar**, in thumb reach.
- **Roster becomes a sticky horizontal strip** of big jersey numbers that stays visible
  while you tap, so you never scroll between "pick player" and "tap stat".
- **Nine thumb-sized stat buttons** in a 3×3 grid — the ones you tap live — each showing
  its running count for the set. The remaining eight (passing, ball-handling, serve in
  play, and the error counterparts) sit behind **More stats**, which remembers whether you
  left it open.
- A full-width **Undo last stat** button.
- Every control is at least 44px — the iOS minimum touch target.

Nothing is duplicated in the data model; it's one app with two layouts.

**This is not yet installable on a phone.** It's a responsive layout you can preview. Making
it a real home-screen app needs a web app manifest and a service worker, and a service
worker requires loading the page once from an HTTPS origin (`file://` and a plain LAN IP
won't register one).

## Keyboard shortcuts (Match tab)

| Key | Stat | | Key | Stat |
|---|---|---|---|---|
| `K` | Kill              | | `T` | Assist |
| `E` | Attack error      | | `Y` | Ball handling error |
| `A` | Kept in play (neither) | | `B` | Block solo |
| `S` | Ace               | | `N` | Block assist |
| `X` | Service error     | | `M` | Block error |
| `W` | Serve in play     | | `D` | Dig |
| `3` `2` `1` `0` | Pass rating / reception error | | `F` | Dig error |
| `Z` | Undo last stat    | | | |

Select a player first — shortcuts record against whoever is highlighted.

## One tap per swing

There is no separate "attempt" button, and you never need one. Attempts are derived:

```
Total Attempts = Kills + Attack Errors + Kept in Play
Hitting %      = (Kills − Attack Errors) / Total Attempts
```

A kill counts itself as an attempt. An error counts itself as an attempt. **`Kept in Play`
(`A`) is only for the third case** — a swing that terminated nothing, because it got dug or
blocked back up. Pressing it alongside a kill would inflate the denominator and understate
the player.

Serves work the same way: `Ace` and `Serve Error` are self-counting, `Serve in Play` is the
remainder.

The readout above the buttons shows the selected player's K / E / attempts / hit% for the
match, updating on every tap, so you can watch attempts climb without recording them.

Hitting % can be negative — more errors than kills — and is shown as `-.083`, red below
zero and green at or above it.
