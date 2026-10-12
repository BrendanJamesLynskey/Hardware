# CLAUDE.md — Hardware index

This repo is an index (README.md table + index.html cards, served by GitHub Pages from `main`)
of presentation series. Each series has a hub repo (e.g. `LLM_Hub_Fourier_Optics_Training`) and
one repo per deck (e.g. `FOptTrn_01_Transforms_in_Training`), each its own Pages site.

## Adding a new series (worked example: Fourier Optics for Training, October 2026)

1. **The user creates the empty repos** from their shell (a cloud session can't create repos):
   ```bash
   gh repo create BrendanJamesLynskey/<Hub_Repo> --public --description "..."
   for r in <Deck_01> <Deck_02> <Deck_03>; do
     gh repo create "BrendanJamesLynskey/$r" --public --description "..."
   done
   ```
   The Claude GitHub App is set to "All repositories", so new repos need no extra access step.
   (Granting app access via `gh api user/installations/...` does not work: it needs a GitHub App token.)
2. **The session attaches them** with `add_repo` (access: push), plus this repo, and clones the
   sister series' hub and decks read-only as templates.
3. **Build to match the sister series**: copy the deck `<head>`/CSS verbatim (only `<title>`
   changes), same hero / slide / concept-chip / footer structure, an analysis script whose
   `results.md` is the source of every number, glossary anchors (`#g-...`) on the hub that decks
   link to, one interactive widget per deck, no horizontal scroll at 375px.
4. **Push** hub and decks straight to `main`. The user then enables Pages:
   ```bash
   for r in <Hub_Repo> <Deck_01> <Deck_02> <Deck_03>; do
     gh api -X POST "repos/BrendanJamesLynskey/$r/pages" -f "source[branch]=main" -f "source[path]=/"
   done
   ```
5. **Index it here**: a card in `index.html` next to its sister series and a row in `README.md`,
   on the session branch; the user merges with
   ```bash
   gh pr create --repo BrendanJamesLynskey/Hardware --base main --head <branch> --title "..." --body "..."
   gh pr merge <branch> --repo BrendanJamesLynskey/Hardware --squash --delete-branch
   ```

Cloud sessions cannot fetch `*.github.io` or `*.vercel.app` (network policy); read a site's
source repo instead.
