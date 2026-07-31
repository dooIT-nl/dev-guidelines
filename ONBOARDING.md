# dooIT × Claude Code — onboarding

Welkom! Bij dooIT gebruiken we **Claude Code** op Odoo.sh met één centrale set
richtlijnen, slash-commands en het dooIT-logo. Dit voorkomt dat iedereen een eigen
`CLAUDE.md` bijhoudt. Deze notitie legt in een paar minuten uit hoe het werkt.

## Het idee in één zin
Alle gedeelde tooling staat in de publieke repo **`dooIT-nl/dev-guidelines`**;
elke dev-repo haalt die op en houdt 'm actueel met **`/sync-guidelines`**.

## Wat er in een dev-repo staat
```
CLAUDE.md                 ← stub: laadt de centrale richtlijnen + JOUW repo-notities
.dooit/guidelines.md      ← gesyncte kopie van de richtlijnen (niet handmatig bewerken)
.dooit/dooit-icon.png     ← het dooIT-logo (voor static/description/icon.png)
.claude/commands/*.md     ← gesyncte slash-commands
```

## 1. Nieuwe repo? Eenmalig "bootstrappen" (= inrichten)
Odoo.sh levert een lege repo op. Draai in de **root** van die repo één keer deze
regel (geen token nodig — de repo is publiek):

```bash
curl -fsSL https://api.github.com/repos/dooIT-nl/dev-guidelines/tarball \
  | tar -xz --strip-components=1 --wildcards '*/commands/*' '*/templates/CLAUDE.md' '*/guidelines.md' '*/assets/*' \
 && mkdir -p .claude/commands .dooit \
 && mv commands/* .claude/commands/ && rmdir commands \
 && mv templates/CLAUDE.md CLAUDE.md && rmdir templates \
 && mv guidelines.md .dooit/guidelines.md \
 && mv assets/* .dooit/ && rmdir assets \
 && git add .claude CLAUDE.md .dooit \
 && git commit -m "chore: bootstrap dooIT tooling" \
 && odoosh-push
```

Herstart daarna je Claude Code-sessie, zodat de nieuwe slash-commands geladen worden.

## 2. Bijblijven: `/sync-guidelines`
Zijn de richtlijnen, commands of het logo centraal gewijzigd? Draai in de repo:

    /sync-guidelines

Dat haalt de laatste versie op en werkt `.dooit/` + `.claude/commands/` bij.
Je repo-specifieke notities in de root-`CLAUDE.md` blijven staan.

## 3. Beschikbare commands
- `/sync-guidelines` — haal de nieuwste richtlijnen, commands en logo op.
- `/vul-register <database> "<partner>"` — scan alle addons en vul het
  Maatwerk register (`docs/customizations.csv`).

## Logo in een nieuwe module
```bash
mkdir -p <module>/static/description
cp .dooit/dooit-icon.png <module>/static/description/icon.png
```

## Spelregels
- **Niet handmatig bewerken:** `.dooit/*` en de gesyncte commands.
- **Repo-/klantspecifieke notities:** onder de markering in de root-`CLAUDE.md`.
- **Pushen op Odoo.sh:** altijd `odoosh-push`, **nooit** `git push`.
- **Nieuwe commands** verschijnen pas na een **herstart** van de sessie.
- **Geen secrets/klantdata** in `dev-guidelines` — die repo is publiek.

## Iets aan de richtlijnen wijzigen?
Bewerk het in **`dooIT-nl/dev-guidelines`** (niet in een klant-repo). Daarna draait
iedereen simpelweg `/sync-guidelines` om mee te gaan.

Vragen? Stel ze in het dev-kanaal. Veel plezier! 🚀
