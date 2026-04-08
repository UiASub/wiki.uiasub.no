---
hide:
  - toc
  - navigation
---
<div class="home-hero">
  <div>
    <p class="home-eyebrow">UiASub knowledge base</p>
    <h1>Everything the team needs, in one clean wiki.</h1>
    <p class="home-lead">
      Browse hardware guides, website documentation, and subsystem notes with a faster,
      clearer landing page built for day-to-day use.
    </p>
    <div class="home-actions">
      <a class="md-button md-button--primary" href="website/">Explore website docs</a>
      <a class="md-button" href="k2zephyr/">Open K2 Zephyr guide</a>
      <a class="md-button" href="topside/">View topside notes</a>
    </div>
    <div class="home-badges">
      <img alt="GitHub last commit" src="https://img.shields.io/github/last-commit/uiasub/wiki.uiasub.no">
      <img alt="GitHub Actions workflow status" src="https://img.shields.io/github/actions/workflow/status/UiASub/wiki.uiasub.no/.github%2Fworkflows%2Fdeploy-mkdocs.yml">
    </div>
  </div>
  <div class="home-panel">
    <img
      src="assets/wiki_uiasub-hero.webp"
      alt="UiASub wiki preview"
      class="home-panel__image"
      width="768"
      height="768"
      decoding="async"
    >
    <div class="home-panel__content">
      <p class="home-panel__title">Quick orientation</p>
      <ul class="home-checklist">
        <li>Use the top navigation to jump between major sections.</li>
        <li>Use search for commands, filenames, and setup notes.</li>
        <li>Open the repository link in the header to edit content.</li>
      </ul>
    </div>
  </div>
</div>

## Start here

<div class="home-card-grid">
  <a class="home-card" href="website/">
    <span class="home-card__eyebrow">Website</span>
    <h3>Frontend and content docs</h3>
    <p>Project structure, JavaScript logic, PWA behavior, and update workflows.</p>
  </a>
  <a class="home-card" href="k2zephyr/">
    <span class="home-card__eyebrow">Firmware</span>
    <h3>K2 Zephyr setup</h3>
    <p>Installation notes and embedded development guidance for the Zephyr stack.</p>
  </a>
  <a class="home-card" href="topside/">
    <span class="home-card__eyebrow">Systems</span>
    <h3>Topside references</h3>
    <p>Operational and subsystem documentation collected for quick team access.</p>
  </a>
</div>

## Local development

<div class="home-steps">
  <div class="home-step">
    <span class="home-step__number">1</span>
    <div>
      <h3>Install dependencies</h3>
      <p>Python is required. Install the MkDocs toolchain once in your environment.</p>
    </div>
  </div>
  <div class="home-step">
    <span class="home-step__number">2</span>
    <div>
      <h3>Run the preview server</h3>
      <p>MkDocs reloads automatically when files in <code>docs/</code> or <code>overrides/</code> change.</p>
    </div>
  </div>
  <div class="home-step">
    <span class="home-step__number">3</span>
    <div>
      <h3>Build before publishing</h3>
      <p>Generate the static output locally to catch broken configuration or content early.</p>
    </div>
  </div>
</div>

```bash
python3 -m pip install mkdocs mkdocs-material mkdocs-awesome-pages-plugin
mkdocs serve
mkdocs build
```

Open the local preview at `http://127.0.0.1:8000/`.

## Editing workflow

- Edit Markdown content in `docs/`.
- Keep navigation updated through the relevant `.pages` file in each folder.
- Use the repository link in the top-right corner to jump straight to source control.
- Push changes when ready; GitHub Actions handles deployment automatically.

## About Material for MkDocs

This site uses [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/setup/), giving the wiki fast navigation, search, theming, and a polished documentation experience on top of MkDocs.
