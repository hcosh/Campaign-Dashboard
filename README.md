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

## Quest States

Each quest has a status selector in the **Quests** tab:

- `Active`
- `Blocked`
- `Completed`
- `Dropped`

Quest state is saved in local storage and shown as a badge on each quest card.

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
