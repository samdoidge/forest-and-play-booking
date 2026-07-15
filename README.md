# Forest & Play — Holiday Club Booking Form

The online booking form for [Forest & Play](https://www.forestandplay.org) holiday club, live at **https://book.forestandplay.org**. Bookings arrive by email at hello@forestandplay.org.

One static file (`index.html`), hosted free on GitHub Pages, submissions delivered by [Web3Forms](https://web3forms.com).

## Changing the holiday dates (the usual job)

1. Open `index.html` and find the block marked **`EDIT HOLIDAY DATES HERE`**
2. Update the `WEEKS` list — one entry per week, listing only the days the club runs
3. Update the dates chip near the top of the page (search for `Summer 2026`)
4. Commit — the live site updates in about a minute

This can be done entirely on github.com with the pencil (edit) button; no tools needed.

## For AI assistants / developers

Read **[CLAUDE.md](./CLAUDE.md)** before changing anything — it documents the architecture, the Web3Forms gotchas (one of which is very non-obvious), the design constraints, and how to test safely.
