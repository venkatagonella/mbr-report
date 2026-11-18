# mbr-report — Cursor Agent Skill

Builds Phenom Weekly/Monthly Business Review (WBR/MBR) HTML decks that match the CRM Core MBR shell (Poppins, sidebar nav, Chart.js CY/PY slides, fiscal Feb–Jan tables, AE exec layouts). **No password gate.**

## Install (consume this skill)

### Option A — personal skills (all projects)

```bash
git clone <REPO_CLONE_URL> ~/.cursor/skills/mbr-report
```

If the folder already exists, update instead:

```bash
cd ~/.cursor/skills/mbr-report && git pull
```

### Option B — project skills (share with anyone who clones the app repo)

```bash
mkdir -p .cursor/skills
git clone <REPO_CLONE_URL> .cursor/skills/mbr-report
```

Restart Cursor or start a new agent chat after installing. Trigger by asking for an **MBR**, **WBR**, or **business review deck**.

## What’s inside

| Path | Role |
|------|------|
| `SKILL.md` | Agent instructions |
| `reference.md` | Slide/chart schema |
| `assets/mbr-template.html` | Full shell (password stripped) |
| `samples/sample-wbr.html` | Compact demo deck |

## Notes

- Optional section comments use a Supabase anon client already present in the shell; re-point `SUPABASE_URL` / `SUPABASE_KEY` if your team uses a different project.
- Do not re-add password protection when generating decks from this skill.
