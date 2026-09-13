# Spot the Landmine

A five-minute browser game for contracting professionals. Six clauses, fifty seconds each — tap the words that will hurt you later.

One file, no dependencies, no backend, no tracking. It works offline once loaded.

## Putting it on GitHub Pages

1. Create a new **public** repository — `spot-the-landmine` is a good name.
2. Upload `index.html` to the root of the repo (drag and drop into the web UI is fine).
3. **Settings → Pages → Build and deployment**. Set *Source* to **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
4. Wait a minute or two. Your game is live at:

   `https://<your-username>.github.io/spot-the-landmine/`

That URL is what the Share button copies, so the link people get is the link that works.

## Editing the content

Everything lives in the `CLAUSES` array near the top of the `<script>` block. Each entry looks like this:

```js
{
  heading: "Limitation of Liability",
  text: "The full paragraph the player reads...",
  mines: [
    { p: "exact substring from text above",
      why: "One or two sentences on why it bites." }
  ],
  missing: "What the clause should have said but doesn't."
}
```

Two rules when adding a clause:

- **`p` must appear in `text` character-for-character, and exactly once.** The game finds landmines by string match. If a phrase appears twice, only the first is used; if it doesn't appear at all, the game logs an error to the console and that landmine silently disappears.
- **Landmines must not overlap.** Two mines can't claim the same words.

Add as many clauses as you like — the game shuffles the library and plays six of them, so a bigger pool means more replay value.

## Tuning the game

Just below the clause library:

| Constant | Default | What it does |
|---|---|---|
| `ROUNDS` | 6 | Clauses played per game |
| `ROUND_TIME` | 50 | Seconds per clause |
| `MINE_POINTS` | 100 | Points per landmine found |
| `WRONG_POINTS` | -25 | Penalty for a wrong tap |
| `WRONG_TIME` | 3 | Seconds lost on a wrong tap |
| `TIME_BONUS` | 4 | Points per second left when a clause is cleared |

The clock stops the moment a landmine is found and stays stopped while the explanation is on screen, so reading costs nothing. Tapping **Keep looking** restarts it. Wrong taps do not pause — that toast clears itself after a couple of seconds. Finding the last landmine in a clause skips the pause and goes straight to the round summary.

`RANKS` sets the score thresholds and the titles. If you widen the clause pool or change the timings, revisit those numbers — a clean run currently lands somewhere around 2,000–2,500.

## Notes

- The wallpaper is an inline SVG doodle tile — bombs, warning triangles, magnifiers, documents, paperclips, gavels, scales and notary stamps. It's applied as a CSS mask, so one drawing re-tints itself for light and dark mode. To change how loud it is, edit the `--doodle` colour in `:root` (and its dark-mode twin); to change the drawings, edit the SVG inside the `body::before` rule.
- The clause text is illustrative drafting written for the game, not from any real agreement.
- Nothing is stored and nothing is sent anywhere. Scores live in the tab until it's closed.
- Dark mode follows the reader's device setting.
