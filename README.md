# 5x1

A volleyball club app for members, events, RSVPs, and attendance.

## Documentation

The product summary is [`docs/product-spec.md`](docs/product-spec.md). The full document package (PRD, TRD, flows, design, plan, backend, testing, and security and privacy) is in [`docs/spec/`](docs/spec/00-README.md). Both are published on GitHub Pages from the `docs/` folder.

## How publishing works

Every approved step is committed and pushed to `main`. The workflow in `.github/workflows/pages.yml` runs on each push that changes `docs/`, and deploys the folder to GitHub Pages.

One-time setup in the repository:

1. Open Settings → Pages.
2. Under "Build and deployment", set Source to **GitHub Actions**.

After that, the site updates automatically on each push.
