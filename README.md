# 🗂️ Workshop Board

A single-file, drag-and-drop kanban board with persistent storage — build
cards, move them between lists, and pick up right where you left off after
a refresh. No frameworks, no build step, no dependencies.

The visual design leans into where the word **"kanban"** actually comes
from: physical signal cards used on Toyota's factory floor to track work
in progress. Cards here are styled as ticket stubs — a punched edge, a
stamped number — set against a kraft-paper board, instead of the usual
flat corporate-blue Trello look.

---

## 🎥 Demo

https://drive.google.com/file/d/1kwYZsbEK2JajWUDK-7CQht6JuPpNV3of/view?usp=sharing

---

## ✨ Features

| Feature | Description |
|---|---|
| **Create cards** | Type into any list's textarea and hit **Add card** (or press `Enter`) |
| **Drag to move** | Reorder within a list, or drag across to another list — a dashed highlight marks the active drop target |
| **Add / delete lists** | Click **+ Add list** to create a column; click `✕` on a list header to remove it (confirms first if it still has cards) |
| **Remove cards** | Hover a card and click the `✕` in its corner |
| **Persistence** | Every change saves instantly to `localStorage` — refresh or close the tab and your board is exactly as you left it |

---

## 🛠️ Tech stack

- **HTML / CSS / vanilla JavaScript** — no framework, no bundler
- Native **HTML5 Drag and Drop API** for card movement
- **`localStorage`** for persistence
- Google Fonts (**Oswald**, **Inter**, **IBM Plex Mono**) for the
  industrial/workshop typography

---

## 🚀 Getting started

No install needed — it's a single static HTML file.

**Option A — just open it**
1. Double-click `kanban-board.html`, or drag it into a browser window.

**Option B — Live Server (recommended)**
1. Open the project folder in VS Code.
2. Install the **Live Server** extension.
3. Right-click `kanban-board.html` → **Open with Live Server**.

Live Server is the better day-to-day option: it serves the file over
`http://localhost` instead of `file://`, and auto-reloads on save while
you're editing.

> ⚠️ **Stick to one method.** `localStorage` is scoped per browser +
> origin, so `file://` and `http://localhost` are treated as *different*
> origins. Switching between them mid-project will make your board look
> empty even though the data is still there under the other origin.

---

## 📁 Project structure

```
.
├── kanban-board.html   # everything: markup, styles, and logic in one file
└── README.md           # this file
```

---

## 🧠 How persistence works

The entire board state — every list and every card — is stored as one
JSON blob under a single `localStorage` key:

```js
const STORAGE_KEY = 'kanban-board-state-v1';
```

- **`loadState()`** runs once on page load, reads that key, and parses it
  back into the in-memory `state` object. If nothing is found (first
  visit, or a different browser/profile), it falls back to three default
  lists: **To do**, **In progress**, **Done**.
- **`saveState()`** runs after every mutation — adding, moving, or
  deleting a card or list — and writes the current `state` straight back
  to `localStorage`.

Because everything funnels through those two functions, swapping the
storage layer later is a small, contained change — for example, pointing
them at a backend API instead of `localStorage` if you want the board to
sync across devices.

**A few notes on `localStorage` itself:**
- It's per-browser, not per-device or per-account — clearing site data or
  switching browsers will not carry your board over.
- Storage is capped (typically ~5–10MB depending on the browser), which is
  far more than a kanban board's text will ever need.

---

## 🎨 Customizing

| Want to change... | Edit... |
|---|---|
| Colors / theme | CSS custom properties at the top of the `<style>` block (`--kraft`, `--orange`, `--blue`, etc.) |
| Default lists on a fresh board | `defaultState()` |
| Card numbering scheme | `state.nextCardNum`, which increments globally and powers the stamped `NO 001` labels |

---

## 🚧 Known limitations

- **No inline editing** — currently the only way to change a card's text
  is to delete it and add a new one. Easy to extend: make the card body
  `contenteditable`, or swap it for a text input on click.
- **No due dates, labels, or assignees** — kept intentionally minimal to
  match the original scope: create, move, persist.
- **Single-user, single-browser** — there's no server or account system,
  so the board only exists in the browser that created it.

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.