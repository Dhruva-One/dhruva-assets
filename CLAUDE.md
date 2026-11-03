# dhruva-assets: notes for agents

A **public** repository (MIT, like dhruva-docs) holding one thing: the AI requirement cards and their screenshots, and
the workflow that imports them as GitHub issues into `Dhruva-One/dhruva-docs` and project 1 of the Dhruva-One org. No
other repository reads from here; `dhruva-docs/Technology/DhruvaOne Repositories and Technology Stack.md` says it "can be
archived once the project cards are imported". The import workflow ran by hand four times on 30 October 2026, all
successful (Actions tab); ask the person before treating the repo as archived or running it again.

## What is here

- `task/cards/ai/cards.json`: 89 cards, one per requirement of `dhruva-docs/Functional/spec/05-ai-dhruva.md` (the IDs
  match one to one). Keys: `id`, `title` (`<ID> · <name>`), `labels`, `pri`, `ph`, `src`, `flag`, `module`, `images`
  and `body` (Markdown; screenshots written as `![caption](@@IMG@@/<file>.jpg)` with the caption in italics below).
- `task/cards/ai/images/`: the 51 JPEG screenshots the cards use, all referenced, none missing (4.7 MB). Desktop shots
  are 1280x800, phone shots 600x1298. Sample data only, and anyone with the link can see them.
- `task/cards/ai/import_cards.py`: Python 3.8+ importer over `gh`. Creates labels, issues and project fields (Phase,
  Priority, Source, Module, Req ID, Flag) and adds each issue to the project. `SETUP.md` is the original browser setup.
- `.github/workflows/import-ai-cards.yml`: "Import AI cards to project", run by hand from the Actions tab
  (`workflow_dispatch`). Inputs: `mode` (`check` tests the token only, `trial` imports AI-CHT-001, `all` every card),
  `update` (rewrite existing issues, default on), `drafts` (project draft items instead of issues). Needs the secret
  `PROJECT_TOKEN` (classic token, `repo` + `project`). Images are linked from
  `raw.githubusercontent.com/Dhruva-One/dhruva-assets/<default branch>/task/cards/ai/images`. The last 60 log lines go
  to the run summary.

## Changing a card or a screenshot

- The spec in dhruva-docs is the source; `cards.json` is a copy. Change the requirement there first (behaviour
  changes go back to the architect), then mirror it here.
- Keep `images[].file` and the `@@IMG@@/` references in `body` the same set; the importer uses only `body`.
- A new image goes in `task/cards/ai/images/` as `.jpg`, named in lower-case kebab-case like the existing ones.
- Labels and the Module field come from the card; a new module needs its option added by hand in the project settings
  (the script prints a note and leaves the field empty otherwise).

## Checks before a PR

There is no CI on pull requests; the only workflow is the manual import. Run these offline from the repo root:
```
python3 -I -m json.tool task/cards/ai/cards.json > /dev/null
python3 -I task/cards/ai/import_cards.py --dry-run     # needs no gh; ends "Finished: 89 created (dry run ...)"
```
`--dry-run` prints the `gh label` and `gh project item-add` calls but not `gh issue create`, and checks no image links.

## Traps

- Images resolve from the **default branch** only: a new screenshot shows in issues after its PR is merged, not before.
- The workflow takes the first `cards.json`, `import_cards.py` and `images` folder that `find` returns anywhere in the
  repo. Do not add a second card set or another `images` folder without changing the workflow.
- Run locally, the script writes `.import-state.json` and `.tmp-body.md` next to itself (there is no `.gitignore`), and
  skips every card in the state file even with `--update`. Never commit them; prefer the workflow.
- The script's defaults are not the workflow's: without `--image-base` it links to `Funtional/Spec/cards/ai/images`
  in dhruva-docs, which does not exist. Its docstring names a `publish_images.sh` that is not in the repo.
- A real run writes to dhruva-docs and the org project, so it is a network action: ask the person first, and start
  with `mode: check`, then `trial`.

## Working with the person

Branch per change; open the PR with `gh` (logged in as ankit-dhruva) or the GitHub connector. CODEOWNERS gives every
file to the architects, @ankit-dhruva and @amitmaheshwari83; the person merges.
