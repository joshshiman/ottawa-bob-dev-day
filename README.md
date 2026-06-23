# Bob Dev Day — Track C

Workshop documentation site for **Track C: AI Government Expense Tracker** — build a
Streamlit web app that extracts and categorizes PDF receipts using IBM Bob and
watsonx.ai Granite 3.

Built as a [Jekyll](https://jekyllrb.com/) site with a custom IBM Carbon Design
System theme and deployed to GitHub Pages.

## Site structure

| Path | Page |
|------|------|
| `index.md` | Home |
| `labs/lab-1-build/` | Lab 1 — Build the Tracker |
| `labs/lab-2-budget/` | Lab 2 — Budget Tracker (bonus) |
| `labs/lab-3-design/` | Lab 3 — Design with Bob (bonus) |
| `labs/cheat-sheet/` | Cheat Sheet — credentials, commands, fixes |
| `resources/` | Workshop downloads — sample invoices, reference solution, requirements, env template (excluded from the build) |

## Run locally

```bash
bundle install
bundle exec jekyll serve
```

Then open <http://localhost:4000/ottawa-bob-dev-day/>.

## Deploy

Pushing to `main` triggers the GitHub Pages workflow
(`.github/workflows/jekyll-gh-pages.yml`). The site publishes to
`https://joshshiman.github.io/ottawa-bob-dev-day/`.

> If the repository name differs, update `baseurl`, `url`, and `repository` in
> `_config.yml` (and the cheat-sheet link in `labs/lab-1-build/README.md`).

---

*Built for IBM Bobathon · Powered by watsonx.ai Granite 3 · Made with Bob*
