---
name: lokalise-create
description: >
  Use this skill when the user wants to create new translation keys in Lokalise with their English text.
  The user is working on a project and wants to register new translation keys directly in Lokalise via CLI
  without touching the local language files (those are managed by pulling from Lokalise).
  Trigger on: "lokalise upload", "create lokalise key", "add key to lokalise", "lokalise publish",
  "/lokalise-create", "push lokalise keys", "add translation key", "create translation key",
  "publish lokalise", "add $t key", "lokalise-create".
---

# Lokalise — Create Keys

Create one or more new translation keys directly in Lokalise via the `lokalise2` CLI.
Do NOT touch local language files — those are managed by pulling from Lokalise separately.

## Key naming convention

Keys are plain names with **no prefix** — the namespace comes from the filename, not the key itself.

- User says `my_button` → key is `my_button`
- User says `item_count` → key is `item_count`

## Translations per key

Each key gets **two** translations:
- `en` → the English text provided by the user
- `keys` (z-keys language) → always the key name itself (e.g. `my_button`)

## Workflow

### 1. Load credentials

Read `.env` from the project root and extract:
- `LOKALISE_TOKEN` — required
- `LOKALISE_PROJECT_ID` — required

If either is missing or empty, stop and tell the user:

> `LOKALISE_TOKEN` and/or `LOKALISE_PROJECT_ID` are not set in `.env`. Please add them and try again.

### 2. Collect key(s) from the user

Ask the user for each key: the **key name** and its **English translation text**.

If the user hasn't provided keys yet, ask:
> What key(s) do you want to create? For each one, provide the key name and the English text.

**Example input formats the user might give:**
```
my_button => Click me
item_count => Number of items
```
Or conversationally: "add `my_button` with English text 'Click me'"

### 3. Confirm before creating

Show a compact one-line summary per key:
```
Creating 2 key(s) in Lokalise (file: default.php):
  my_button   → "Click me" / keys: "my_button"
  item_count  → "Number of items" / keys: "item_count"

Proceed? [y/N]
```

Wait for confirmation.

### 4. Create the keys via CLI

For each key, run:

```bash
lokalise2 key create \
  --token "$LOKALISE_TOKEN" \
  --project-id "$LOKALISE_PROJECT_ID" \
  --key-name "<KEY>" \
  --platforms "web" \
  --filenames '{"web":"lang/%LANG_ISO%/default.php"}' \
  --translations '[{"language_iso":"en","translation":"<ENGLISH_TEXT>"},{"language_iso":"keys","translation":"<KEY>"}]'
```

> **Note:** Adjust `--filenames` to match your project's language file path convention.

Important: `--key-name` takes a **plain string** (e.g. `widget_dark_monochrome`), NOT a JSON object.

Show progress: `✓ my_button`, `✗ item_count (error: ...)`

After all keys are processed, report total created vs failed.

### 5. Build (optional)

Only run a build command if the user explicitly asks for it — don't run it automatically.

## Notes

- Key name is plain (e.g. `my_button`) — no prefix. The namespace comes from the filename
- `en` translation = English text provided by the user
- `keys` translation = always the key name itself
- `platforms` is always `["web"]`
- Never modify local language files — they are pulled from Lokalise separately
- `lokalise2` CLI is assumed to be available in PATH. If `lokalise2` is not found, try `~/bin/lokalise2`
- `.env` is read from the project root; adjust path if needed for your project structure
