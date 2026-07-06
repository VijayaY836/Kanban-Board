# Workshop board

A single-file, drag-and-drop kanban board with cards that persist across
refreshes using `localStorage`. No build step, no dependencies — open the
HTML file and it works.

The visual theme leans into where "kanban" actually comes from: Toyota's
physical signal cards on the factory floor. Cards render as ticket stubs
with a punched edge and a stamped number, set against a kraft-paper board.

## Features

- **Create cards** — type into the textarea at the bottom of any list and
  hit **Add card** (or press `Enter`).
- **Move cards** — drag a card to reorder it within a list, or drop it onto
  another list to move it across. A dashed highlight shows the active drop
  target.
- **Add / delete lists** — click **+ Add list** to create a new column;
  click the `✕` on a list header to remove it (you'll be asked to confirm
  if it still has cards in it).
- **Remove cards** — hover a card and click the `✕` in its corner.
- **Persistence** — every change is written to `localStorage` immediately.
  Closing the tab or refreshing the page brings your board back exactly as
  you left it.

## Running it

No install needed — it's a single static HTML file.

1. Open `kanban-board.html` directly in a browser (double-click it, or
   drag it into a browser window), **or**
2. In VS Code, install the **Live Server** extension, right-click
   `kanban-board.html`, and choose **Open with Live Server**.

Live Server is the better option day-to-day: it auto-reloads on save while
you're editing, and serves the file over `http://localhost` rather than
`file://`, which some browser APIs prefer.

## How persistence works

The board's entire state (all lists and cards) is stored as a single JSON
blob under one `localStorage` key:

```js
const STORAGE_KEY = 'kanban-board-state-v1';
```

- `loadState()` runs once on page load, reads that key, and parses it back
  into the in-memory `state` object. If nothing is found (first visit, or
  a different browser/profile), it falls back to three default lists —
  **To do**, **In progress**, **Done**.
- `saveState()` runs after every mutation (add/move/delete a card or list)
  and writes the current `state` straight back to `localStorage`.

Because everything funnels through those two functions, persistence is
easy to swap out later — for example, pointing `saveState`/`loadState` at
a backend API instead of `localStorage` if you want the board to sync
across devices.

### Notes on localStorage

- It's scoped per browser + origin. Opening the file via `file://` in
  Chrome vs. Firefox, or via `file://` vs. `http://localhost`, are treated
  as different origins — so your board may look empty if you switch how
  you're opening the file. Stick to one method (ideally Live Server) for
  consistent persistence.
- It's per-browser, not per-device or account. Clearing site data / browser
  cache will wipe the board.
- Storage is capped (usually ~5–10MB depending on the browser), which is
  far more than a kanban board's worth of text will ever need.

## File structure

```
kanban-board.html   — everything: markup, styles, and logic in one file
README.md           — this file
```

## Customizing

A few easy places to start if you want to make it your own:

- **Colors** — all theme colors are CSS custom properties at the top of
  the `<style>` block (`--kraft`, `--orange`, `--blue`, etc.). Change the
  values there and the whole board updates.
- **Default lists** — edit the `defaultState()` function to change what a
  brand-new board starts with.
- **Card numbering** — `state.nextCardNum` increments globally every time
  a card is created; it's what powers the stamped `NO 001` labels.

## Known limitations

- No editing of existing card text after creation — currently the only
  way to change a card is to delete it and add a new one. Easy to add if
  you want it: turn the card body into a `contenteditable` element or an
  inline text input on click.
- No due dates, labels, or assignees — kept intentionally minimal per the
  original brief (create, move, persist).
- Single-user, single-browser only, since it's backed by `localStorage`
  rather than a shared database.