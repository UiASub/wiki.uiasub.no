# UiASub Wiki

Welcome. Choose a path:

- **Read the wiki**: Use the navigation menu to browse topics, or use the search box to jump to a page.
- **Edit the wiki**: Follow the quick-start below to run the site locally and make changes.

## Read the wiki

Start with the navigation menu to the top to browse sections. If you know what you are looking for, use the search box to jump directly to a page.

![GitHub last commit](https://img.shields.io/github/last-commit/uiasub/wiki.uiasub.no)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/UiASub/wiki.uiasub.no/.github%2Fworkflows%2Fdeploy-mkdocs.yml)

## Edit the wiki (MkDocs quick-start)

These steps follow the [official MkDocs getting started guide](https://www.mkdocs.org/getting-started/)

1. Install MkDocs (Python required):
```bash
pip install mkdocs
```
2. Serve the site locally from this repo:
   ```bash
   mkdocs serve
   ```
3. Open the local preview:
   ```url
   http://127.0.0.1:8000/
   ```
4. Edit Markdown files in `docs/` (including this file at `docs/index.md`).
   MkDocs reloads the site when you save.

When you're ready to publish, push the site and Github Actions will build it automatically

## Material for MkDocs

Material for MkDocs is a powerful documentation framework on top of MkDocs, a static site generator for project documentation.

Install with:

```bash
pip install mkdocs-material
```

See more functionality here: <https://squidfunk.github.io/mkdocs-material/setup/>
