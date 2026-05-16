# Thesis Review Portal (GitHub-native)

This repository contains a GitHub-native pipeline that compiles a LaTeX thesis using Tectonic via GitHub Actions and publishes an interactive review portal on GitHub Pages with Hypothesis annotations for supervisor collaboration.

**Architecture**

Local LaTeX
      ↓
Git Push
      ↓
GitHub Actions
      ↓
PDF Compilation (Tectonic)
      ↓
GitHub Pages (gh-pages)
      ↓
Hypothesis (embed)
      ↓
Supervisor comments

Repository layout

/
├── thesis.tex
├── chapters/
├── figures/
├── bibliography.bib
├── .github/
│   └── workflows/
│       └── build.yml
├── public/
│   ├── index.html
│   └── thesis.pdf (generated)
└── README.md

Getting started
-----------------

1. Write your thesis locally in LaTeX and commit to `main` branch.
2. Push to GitHub.
3. GitHub Actions (see `.github/workflows/build.yml`) runs and compiles `thesis.tex` using Tectonic.
4. The generated `thesis.pdf` is copied to `public/` and deployed to the `gh-pages` branch using `peaceiris/actions-gh-pages`.
5. Open the Pages site (repository → Settings → Pages) to view the portal.

Setup & configuration
----------------------

- Enable GitHub Pages: go to your repository Settings → Pages and set the source to the `gh-pages` branch (root). After first deploy it will show the URL.

- Workflow permissions:
  - Go to Settings → Actions → General and ensure workflows have permission to create and update Pages. The workflow uses the `GITHUB_TOKEN` with `pages: write` permission.

- Create Hypothesis account and groups:
  1. Sign up at https://hypothes.is/.
  2. Create a private group for your supervisors (Account → Groups → New Group).
  3. Share the group link with supervisors so they can select the group before annotating.

Using the portal
------------------

- Open the Pages URL for your repository. The portal displays the compiled PDF.
- Select text inside the PDF (click & drag) and the Hypothesis annotation popup appears.
- Choose your private group to keep annotations limited to supervisors, or use your account-only private annotations.

Troubleshooting
---------------

- If the workflow fails to compile:
  - Check the Actions run log (Actions → select workflow run → Logs).
  - Download the build logs created by `xu-cheng/latex-action` (kept in the workspace) and inspect errors.
  - Common issues:
    - Missing package: Install via TeX Live package names or vendor the `.sty` file into your repo.
    - Broken includes: verify `\input{}` and file paths (use `chapters/` as relative paths).
    - Bibliography issues: ensure `.bib` entries are valid and `\bibliography{bibliography}` is present. Tectonic generally runs BibTeX automatically.

- If `thesis.pdf` is not visible on Pages:
  - Ensure the workflow successfully copied `thesis.pdf` to `public/` (check Action logs).
  - Confirm `peaceiris/actions-gh-pages` deployment succeeded and created `gh-pages` branch.
  - Ensure Pages site is configured to use `gh-pages` branch as the source.

Common LaTeX build fixes
-------------------------

- Fix missing fonts or packages by vendoring `.sty` files into the repo's `texmf/` or including them next to the root file.
- For bibliography errors, run a local tectonic compile to reproduce: `tectonic --print_logs thesis.tex`.
- For cross-reference warnings, re-run build locally or rely on the action which runs the necessary passes.

Security & privacy notes
-------------------------

- The Hypothesis client is loaded from `https://hypothes.is/embed.js`. If your supervisors must use private groups, ensure they join the private group before annotating.
- The PDF and annotations are public if your Pages site is public. To restrict access, make the repository private and use a paid GitHub Pages plan or alternative hosting that supports authentication (out of scope). Hypothesis groups can still be private within a public PDF but note metadata visibility.

Deployment instructions (quick)
----------------------------

1. Push to `main`.
2. Wait for Actions to complete.
3. Confirm `gh-pages` branch created and `public/index.html` + `public/thesis.pdf` present there.
4. Visit the Pages URL and test annotation flow.

Optional improvements
---------------------

- Use PDF.js for improved text-layer selection and better cross-browser PDF selection.
- Add a version selector UI driven by Git tags or commit SHAs.
- Show commit metadata beside the PDF (author, commit message, timestamp).
- Support multiple thesis versions and chapter previews.
- Add dark mode and annotation analytics.

Support
-------
If you want, I can:

- Convert the iframe into a PDF.js viewer with embedded Hypothesis text-layer integration.
- Add a version selector and automated changelog on the portal.
# master-thesis
