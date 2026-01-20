---
hide:
  - toc
  - navigation
---
# UiASub Wiki

![Read the wiki](assets/wiki_uiasub.png){ width="450" align=right }

![GitHub last commit](https://img.shields.io/github/last-commit/uiasub/wiki.uiasub.no)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/UiASub/wiki.uiasub.no/.github%2Fworkflows%2Fdeploy-mkdocs.yml)

Welcome.

Start with the navigation menu to the top to browse sections. If you know what you are looking for, use the search box to jump directly to a page.

## Edit the wiki (MkDocs quick-start)

Top right of this webpage is the official repo for this wiki.

These steps follow the [official MkDocs getting started guide](https://www.mkdocs.org/getting-started/)

1. Install MkDocs requrements (Python required):
```bash
pip install mkdocs
pip install mkdocs-material
pip install mkdocs-awesome-pages-plugin
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
5. Add new folders and files in their respective `.pages` file so it is navigable, remember to add a `.pages` file to a new folder.

When you're ready to publish, push the site and Github Actions will build it automatically

## Material for MkDocs

Material for MkDocs is a powerful documentation framework on top of MkDocs, a static site generator for project documentation.

See more functionality here: <https://squidfunk.github.io/mkdocs-material/setup/>
