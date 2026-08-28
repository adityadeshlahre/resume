# AGENTS.md

## Build

```bash
pdflatex -output-directory tempResume -interaction=nonstopmode resume.tex \
  && pdflatex -output-directory tempResume -interaction=nonstopmode resume.tex \
  && rm -f tempResume/resume.aux tempResume/resume.log tempResume/resume.out tempResume/resume.toc
```

Run twice for cross-references. Output goes to `tempResume/resume.pdf`. Do not run `pdflatex` without `-output-directory tempResume` — it dumps `.aux`/`.log` into the repo root.

## CI

- **build.yml**: Triggers on push to `master`. Builds `resume.tex` via `fudan-mse/latex-action` and uploads artifact.
- **release.yml**: Triggers on GitHub release publish. Builds resume, uploads PDF + `.tex` to the release, deploys PDF to GitHub Pages via `JamesIves/github-pages-deploy-action`.

Default branch is `master`, not `main`. Make changes directly on `master` — no git worktree needed (overrides global rule).

## Files

- `resume.tex` — the resume. Edit this.
- `cover-letter.tex` — generic cover letter template with role-specific swap-in paragraphs (commented at bottom).
- `tempResume/` — build output directory, gitignored. Contains variant PDFs and `.tex` files (phone, frontend, backend, anonymized, etc.) that are not built by CI.

## LaTeX conventions

- Uses `\pdfgentounicode=1` for ATS-readable PDFs.
- Custom commands: `\resumeSubheading`, `\resumeItem`, `\resumeProjectHeading`, `\resumeSubSubheading`. Use these, not raw tabular environments.
- Sections are wrapped in `\resumeSubHeadingListStart` / `\resumeSubHeadingListEnd`.
