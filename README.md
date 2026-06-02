## Webxdc Word Challenge

"Word Challenge" is not one challenge per day. I thought of that, but then if two people send the app in one day, the leaderboard is split up. For this reason, it's just based on random seed generation. Each app is it's own unique challenge.

The group chat is only notified when someone takes first place on the leaderboard (the first person to complete the challenge doesn't notify group). Other updates are sent to chat but just don't pop a notification.

Result grids are included in the leaderboard now. No need to manually paste them anywhere.

The app will also show you when other people are also solving or even if they're just in the app looking at the leaderboard. No reason other than to delight the user and make people feel more connected.

### Stats

Possible Guesses: 4,783
Possible Answers: 2,309

Slurs removed entirely. Unseemly words are guessable, but never the answer.

App size: 30 KB

## Daily Word (`daily.html`)

`daily.html` is a different take that fixes the "40 apps in one chat" problem. Instead
of one app per challenge, it's a **single app that lives in the chat forever** and
serves a **new word every day**:

- **Same word for everyone, every day.** The day's word is derived deterministically
  from the calendar date (a seeded shuffle of the answer pool), so every device picks
  the same word with zero coordination — no challenge negotiation, no rolling/meta
  messages. No word repeats for ~2,300 days.
- **The first open sets everything up.** When the app is opened for the first time,
  that player:
  - **picks the rollover timezone** (defaulting to their own) — which timezone's
    midnight the puzzle turns over on. DST is handled automatically (the boundary
    tracks the zone, not a fixed offset).
  - That first day becomes **Daily #1**, and you can't browse to days before the app
    existed.
  - A **random word-seed** is generated for this instance. The choice is broadcast as
    a `config` update (timezone + seed + first day) and everyone in that chat follows
    it, so the whole group is always on the same puzzle. If two people set up at once,
    the config with the lowest update serial wins for everyone, so they can't diverge.
- **Each launched instance is independent.** Because the word-seed is random per
  instance, two copies of the app started in the same chat (even on the same day) run
  **different word sequences** and number from their own Day 1 — they're separate
  games with separate leaderboards, not duplicates.
- **Two tabs.** *Daily* shows today's board + today's leaderboard. *Overall* aggregates
  every day into a running scoreboard — so you can finally see who's winning the most.
  - Overall scoring: each solved puzzle = `7 − guesses` points (1 guess = 6 pts … 6 = 1).
  - 🥇 = days you topped the daily board; 🔥 = current solve streak.
  - Ties break on points → golds → wins → fewest total guesses → time.
- **Browse history.** `‹` / `›` next to the date page back through past days to see each
  day's leaderboard and reveal old answers (you can only play *today*).
- Keeps everything the original has: live "who's here" presence, mini result grids,
  chat notification only when someone takes the daily lead, and the per-device resume /
  finished-on-another-device handling.

The original `index.html` is untouched.

### Shipping it as the app

A webxdc `.xdc` uses **`index.html`** as its entry point. To package *Daily Word* as the
app, rename/copy `daily.html` to `index.html` in the bundle (and set
`manifest.toml`'s `name` to `Daily Word`). Keep the original around under a different
name if you still want both.
