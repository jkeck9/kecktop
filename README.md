# Stonetop

A shared reference for the table. Players can read it from any phone or laptop; you're the only one who edits it.

## Files

| File | What it is |
|---|---|
| `stonetop.json` | **The only file you edit.** All villagers, ties, locations. |
| `index.html` | The app. Don't touch unless you want to change how it looks. |
| `d3.min.js` | Draws the relationship graph. Never touch. |

## Putting it online (free, no billing, ever)

1. Make a GitHub account.
2. Create a new repository — name it whatever, set it to **Public**.
3. Upload all three files (drag and drop onto the repo page).
4. Go to **Settings → Pages**. Under "Branch," pick `main` and `/ (root)`. Save.
5. Wait ~60 seconds. Your URL appears at the top of that page — something like `https://yourname.github.io/stonetop/`. Send it to the players.

To update after a session: open `stonetop.json` on GitHub, click the pencil icon, edit, click **Commit changes**. Live in about 30 seconds. If you break the syntax, GitHub's editor underlines it in red before you can save.

## Adding a villager

Copy an existing block and change it. Every villager needs `id`, `name`, `pc`, `role`, `home`, `notes`, `ties`:

```json
{
  "id": "maderyn",
  "name": "Maderyn",
  "pc": false,
  "role": "Weaver",
  "home": null,
  "notes": "Keeps a loom by the north wall. Deaf in one ear.",
  "ties": [
    { "to": "alwyn", "label": "married to" }
  ]
}
```

Rules that matter:

- **`id`** is lowercase, no spaces. It's the app's internal handle; nobody sees it. Once other villagers reference it in their ties, don't change it.
- **`pc`** is `true` or `false` (no quotes). PCs get an orange stripe and a bigger dot on the graph.
- **`home`** is either `null` or the `id` of a location. Leave it `null` for now.
- **`notes`** can be empty (`""`).
- **Commas** go between entries but never after the last one. This is the #1 thing that breaks JSON.

## Ties — enter each one ONCE

If you write `married to` on Brogan pointing at Egraine, Egraine's card automatically says `married to Brogan`. Don't add it to both — you'll get a doubled line on the graph.

The app knows how to reverse these labels:

| You write | The other person's card shows |
|---|---|
| `married to` | married to |
| `father of` / `mother of` | child of |
| `commands` | serves under |
| `mentors` | apprenticed to |
| `taught` | taught by |
| `works with` | works with |
| `works for` | employs |
| `smitten with` | admired by |

Any label not on that list gets mirrored as-is — so `"rivals"` shows as `rivals` on both cards, which is usually what you want. To add a new reversible pair, find `const INVERSE` in `index.html` and add a line.

## Not built yet

- **Homes / settlement map** — the `locations` list and `home` field are wired up and waiting. Needs the village map image and a click-to-place tool.
- **Steading** — tab exists, is a placeholder. Pop, surplus, defenses, upgrades.
- **World map** — the `world` key is sitting empty for whenever you want it.
