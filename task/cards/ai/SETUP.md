# dhruva-assets: setup (about 5 minutes, all in the browser)

1. Create the repository: GitHub > Dhruva-One > New repository. Name `dhruva-assets`, visibility **Public** (this is what lets screenshots show inside issues), tick "Add a README".
2. Upload the files: open the repository > Add file > Upload files. Drag in everything from this folder: the `images` folder, `cards.json`, `import_cards.py` (53 files). Commit.
3. Add the workflow: Add file > Create new file, name it `.github/workflows/import-ai-cards.yml` (typing the slashes creates the folders), paste the contents of the file of the same name from this package, Commit.
4. Add the token: Settings > Secrets and variables > Actions > New repository secret. Name `PROJECT_TOKEN`, value a classic personal access token with scopes `repo` and `project` (Profile > Settings > Developer settings > Personal access tokens > Tokens (classic)). If the org uses SSO, click Configure SSO on the token and authorise Dhruva-One.
5. Run: Actions tab > Import AI cards to project > Run workflow.
   - First mode `check`, then `trial` (one card, AI-CHT-001: open it and confirm the screenshot shows), then `all`.
   - Leave **update** ticked: cards already imported are rewritten with working screenshots, no duplicates.

Note: the 51 screenshots become publicly viewable by anyone with the link. They use sample data only.
