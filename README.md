# lokalise-create — Claude Skill

A Claude Code skill for creating translation keys directly in [Lokalise](https://lokalise.com) via the `lokalise2` CLI — without touching local language files.

## What it does

When triggered, this skill will:
1. Read `LOKALISE_TOKEN` and `LOKALISE_PROJECT_ID` from your project's `.env`
2. Ask you for key name(s) and their English translations
3. Show a confirmation summary
4. Create the keys in Lokalise via the CLI (with `en` + `keys` translations)

## Requirements

- [`lokalise2` CLI](https://github.com/lokalise/lokalise-cli-2-go) installed and available in `PATH` (or at `~/bin/lokalise2`)
- A `.env` file in your project root with:
  ```
  LOKALISE_TOKEN=your_token_here
  LOKALISE_PROJECT_ID=your_project_id_here
  ```

## Install

```bash
# Clone into your Claude skills directory
git clone https://github.com/FrankIglesias/lokalise-create-skill.git \
  ~/.claude/skills/lokalise-create
```

Or register as a plugin marketplace and install from Claude Code:

```
/plugin marketplace add FrankIglesias/lokalise-create-skill
```

## Usage

Open Claude Code in your project and say any of:

- `create lokalise key`
- `add translation key`
- `add $t key`
- `/lokalise-create`

Claude will guide you through the rest.

## Customising the file path

By default the skill uses `lang/%LANG_ISO%/default.php` as the `--filenames` value in the CLI call. Edit `SKILL.md` to match your project's actual language file convention.

## License

MIT
