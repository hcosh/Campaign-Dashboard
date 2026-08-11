# Novalis Campaign Dashboard

## Run Locally (macOS)

1. Open Terminal.
2. Go to the project folder:
   ```bash
   cd "/Users/hankcoshnear/Documents/Code/D&D Dashboard"
   ```
3. Start a local server:
   ```bash
   python3 -m http.server 8080
   ```
4. Open the dashboard:
   `http://localhost:8080/Novalis%20Campaign%20Shared.html`

You can also double-click `Novalis Campaign Shared.html`, but serving via `http://localhost` is more reliable for browser features.

## Notes System

Use the **Notes** tab to store campaign updates.

- `Session Label` is optional and can be values like `Session 7` or `Travel Day`.
- `Tags` accepts comma-separated values like `combat, clue, lore, npc`.
- `Assign Quest` has three modes:
  - `Auto-detect from note text`
  - `Keep unassigned`
  - Explicit quest selection
- `Search Notes` filters across note text, session labels, tags, and quest names.
- `Save Note` stores notes in browser local storage.

Saved notes appear in three places:

- Under matching quest cards in the **Quests** tab (with note count badge).
- In **Unassigned Notes** when no confident quest match is found.
- In **Session Timeline** sorted by newest first.

You can unassign a wrongly attached note directly from the quest card, then reassign it from **Unassigned Notes**.

## DM Recaps

Use the **Recaps** tab to process full DM session recaps.

- Add `Session Label` and optional `In-World Date`.
- Paste recap text and click `Process Recap`.
- Optional routing markers at the start of a line:
   - `Quest:`
   - `NPC:`
   - `Lore:`
   - `Timeline:`

Routing priority:

1. Marker-based routing
2. Exact quest/NPC match
3. Keyword scoring
4. Fallback to **Review Queue**

Low-confidence or unmatched recap fragments are placed in **Review Queue**, where you can:

- Assign to quest
- Add to lore with a selected category
- Send to timeline
- Discard

NPC-matched recap updates appear as short recap update blocks in the NPC panel.

Processed recaps are also listed in **Recent Recaps** with routed and queued counts for quick review.

## Lore Codex

Use the **Lore** tab for reference entries and imports.

- Categories:
   - Religion
   - History
   - Factions
   - Locations
   - Organizations
   - Items
   - Events
   - Customs/Culture
- Add entries manually with title, category, tags, body, optional linked quest, and optional linked NPC.
- Existing lore entries can be edited or deleted directly from the lore list.
- Search supports title, body, category, tags, source, and linked entities.
- Filters support category and linked NPC.

### Lore Import

- `Import Lore` accepts `.txt`, `.md`, and `.json`.
- Text/markdown imports are chunked and category-inferred.
- Low-confidence imports are sent to **Review Queue**.
- JSON imports must be valid lore payloads.

### Lore Export

- `Export Lore` downloads all lore entries as JSON backup.

## Quest States

Each quest has a status selector in the **Quests** tab:

- `Active`
- `Blocked`
- `Completed`
- `Dropped`

Quest state is saved in local storage and shown as a badge on each quest card.

## Maps

Use the **Maps** tab to view map images stored in the repo.

- The dashboard currently loads repo-root image files directly, starting with the Thundertree player map.
- Open the tab from a local server so the browser can load the image files reliably.
- To add more maps, place the image file in the repo and register it in the dashboard's map library.

## Auto-Matching Rules

Assignment happens in this order:

1. Manual quest selection (if chosen)
2. Exact quest title found in note text
3. Quest keyword scoring
4. Fallback to `Unassigned`

Notes also store assignment metadata (`manual`, `title`, `keyword`, `unassigned`) and confidence (`high`, `medium`, `low`).

## Backup and Restore

In the **Notes** tab:

- `Export Notes JSON` downloads all notes.
- `Import Notes JSON` restores from an export file.

Recommended: export after every session as a backup.

Also recommended:

- Export lore JSON after major world-building updates.

## Shared Campaign JSON (Repo Sync)

Use repository sync to share campaign state with other people using this dashboard.

You can also connect the dashboard directly to the repo file so changes write back automatically.
That browser-based auto sync requires a File System Access capable browser such as Chrome or Edge, and it works best when the app is opened from `http://localhost`.

### Files

- Shared file in repo root: `campaign-shared.json`

### In-app controls

In the **Recaps** tab under **Campaign Sync (Shared JSON)**:

- `Export Shared JSON`: exports all shared dashboard state as `campaign-shared.json`.
- `Import Shared JSON`: imports a shared campaign bundle from disk.
- `Load Repo JSON`: loads `campaign-shared.json` from the current folder via HTTP.
- `Connect Auto Sync`: picks `campaign-shared.json` once and keeps writing changes back to it automatically.

### What is included in shared campaign JSON

- Quest states
- Notes
- Lore entries
- Recaps
- Review queue

### Team workflow

1. Everyone pulls latest repo changes:
   ```bash
   git pull
   ```
2. DM or editor updates dashboard and clicks `Export Shared JSON`.
3. Replace the repo file with the exported `campaign-shared.json`.
4. Commit and push:
   ```bash
   git add campaign-shared.json "Novalis Campaign Shared.html" README.md
   git commit -m "Update shared campaign state"
   git push
   ```
5. Other users pull and click `Load Repo JSON` to sync locally.

If you use `Connect Auto Sync`, you only need to choose the repo file once per browser profile; after that, saves from the dashboard update `campaign-shared.json` automatically.

Note: `Load Repo JSON` requires running from a local server (for example `python3 -m http.server 8080`), not opening the HTML directly from Finder.

## Suggested Session Flow

1. Update quest states at the start of prep.
2. During play, save notes with a short session label and optional tags.
3. At session end, review **Unassigned Notes** and reassign anything ambiguous.
4. Export notes JSON as backup.

## Suggested Git Workflow

After each session update:

1. Check status:
   ```bash
   git status
   ```
2. Stage files:
   ```bash
   git add "Novalis Campaign Shared.html" README.md
   ```
3. Commit with session context:
   ```bash
   git commit -m "Session 7 notes and quest updates"
   ```

Optional: use branches for larger dashboard changes.
