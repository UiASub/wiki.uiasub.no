---
hide:
  - toc
  - navigation
---
<div class="home-hero">
  <div>
    <p class="home-eyebrow">UiASub Documentation</p>
    <h1>UiASub Wiki</h1>
    <p class="home-lead">
      Technical documentation, hardware guides, and operational notes for the UiASub project.
    </p>
    <div class="home-actions">
      <a class="md-button" href="website/">Website Docs</a>
      <a class="md-button" href="k2zephyr/">K2 Zephyr Docs</a>
      <a class="md-button" href="topside/">Topside Docs</a>
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
      <p class="home-panel__title">Wiki Usage</p>
      <ul class="home-checklist">
        <li>Navigate categories using the top menu.</li>
        <li>Search for specific commands, files, or notes.</li>
        <li>Edit content via the repository link in the header.</li>
      </ul>
    </div>
  </div>
</div>

## Documentation Categories

<div class="home-card-grid">
  <a class="home-card" href="website/">
    <span class="home-card__eyebrow">Website</span>
    <h3>Frontend & Content</h3>
    <p>Project structure, logic, PWA configuration, and content management.</p>
  </a>
  <a class="home-card" href="k2zephyr/">
    <span class="home-card__eyebrow">Firmware</span>
    <h3>K2 Zephyr</h3>
    <p>Installation instructions and embedded development guides for Zephyr.</p>
  </a>
  <a class="home-card" href="topside/">
    <span class="home-card__eyebrow">Systems</span>
    <h3>Topside</h3>
    <p>Subsystem references and operational documentation for the topside.</p>
  </a>
</div>

## Editing the wiki

<div class="home-steps">
  <div class="home-step">
    <span class="home-step__number">1</span>
    <div>
      <h3>Install Dependencies</h3>
      <p>Install the MkDocs toolchain and required plugins using Python.</p>
    </div>
  </div>
  <div class="home-step">
    <span class="home-step__number">2</span>
    <div>
      <h3>Serve Locally</h3>
      <p>Run the MkDocs development server with auto-reload enabled.</p>
    </div>
  </div>
  <div class="home-step">
    <span class="home-step__number">3</span>
    <div>
      <h3>Build Output</h3>
      <p>Test the static build locally before deploying to production.</p>
    </div>
  </div>
</div>

### Install Dependencies

```bash
python3 -m pip install mkdocs mkdocs-material mkdocs-awesome-pages-plugin
```

### Serve locally

```bash
mkdocs serve
```

Open the local preview at `http://127.0.0.1:8000/`.

### Build Output

Test the static build locally before deploying to production.

```bash
mkdocs build
```

When pushing to github it automatically builds and releases the webpage.

### Release

- Edit Markdown files within the `docs/` directory.
- Update the `.pages` files to modify navigation.
- Click the repository icon in the header to edit directly in source control.
- Push to `main`; GitHub Actions will automatically build and deploy.

## Framework

Built with Material for MkDocs.
