# dooIT — dev-guidelines

Centrale bron voor onze **Claude Code**-tooling: de gedeelde development-richtlijnen
én de slash-commands. Eén bron van waarheid, zodat niemand meer een eigen kopie
van `CLAUDE.md` hoeft bij te houden.

## Inhoud van deze repo

| Pad | Wat |
|-----|-----|
| `guidelines.md` | De dooIT development-richtlijnen (wordt in dev-repos ingeladen als `.dooit/guidelines.md`). |
| `templates/CLAUDE.md` | De 2-regel stub die in de root van elke dev-repo komt. |
| `commands/*.md` | De gedeelde slash-commands (`/sync-guidelines`, `/vul-register`, …). |

## Hoe het werkt

Elke dev-repo bevat:

```
CLAUDE.md                 ← stub: laadt @.dooit/guidelines.md + repo-specifieke notities
.dooit/guidelines.md      ← gesyncte kopie van guidelines.md (niet handmatig bewerken)
.claude/commands/*.md     ← gesyncte slash-commands
```

- **Wijzigen van richtlijnen/commands:** bewerk het hier, in `dooIT-nl/dev-guidelines`.
- **Ophalen in een dev-repo:** draai `/sync-guidelines` in die repo.

## Nieuwe repo opzetten (eenmalige bootstrap)

Odoo.sh levert een lege repo op. Draai in de **root van de nieuwe repo** één keer
onderstaande regel. Die haalt de tooling op, zet de stub-`CLAUDE.md` neer en commit:

```bash
gh api repos/dooIT-nl/dev-guidelines/tarball -H "Accept: application/vnd.github+json" \
  | tar -xz --strip-components=1 --wildcards '*/commands/*' '*/templates/CLAUDE.md' '*/guidelines.md' \
  && mkdir -p .claude/commands .dooit \
  && mv commands/* .claude/commands/ && rmdir commands \
  && mv templates/CLAUDE.md CLAUDE.md && rmdir templates \
  && mv guidelines.md .dooit/guidelines.md \
  && git add .claude CLAUDE.md .dooit \
  && git commit -m "chore: bootstrap dooIT tooling" \
  && odoosh-push
```

> Voorwaarde: de `gh` CLI is ingelogd met toegang tot `dooIT-nl` (`gh auth status`).
> Geen `gh`? Zet een `GITHUB_TOKEN` (Odoo.sh environment variable, nooit in code)
> en gebruik de `curl`-variant, of log in met `gh auth login`.

Na de bootstrap zijn `/sync-guidelines` en `/vul-register` beschikbaar
(evt. na een herstart van de Claude Code-sessie). Daarna houd je alles bij met
**alleen** `/sync-guidelines`.

## Spelregels

- Bewerk **nooit** `.dooit/guidelines.md` of de gesyncte commands in een dev-repo —
  die worden overschreven. Wijzig het hier centraal.
- Repo-/klantspecifieke notities: onder de markering in de repo-root `CLAUDE.md`
  (die blijft bij een sync behouden).
- Pushen op Odoo.sh: altijd `odoosh-push`, nooit `git push`.
