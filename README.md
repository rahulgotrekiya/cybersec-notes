# Cyber Security Notes

My cybersecurity study notes, published with [Quartz v5](https://quartz.jzhao.xyz).

**Live site:** https://rahulgotrekiya.github.io/cybersec-notes

| | |
|---|---|
| `content/` | the notes (copied from my Obsidian vault — **not** edited here) |
| `quartz.config.yaml` | site config |
| `publish` | re-copy the vault and push |
| `.github/workflows/deploy.yml` | builds and deploys on every push to `v5` |

## Publishing a change

Notes are written in Obsidian. `content/` is a copy, so after editing:

```bash
./publish "what changed"
```

That rsyncs the vault into `content/`, commits, and pushes. Actions does the rest.

## Local preview

```bash
npm ci
npx quartz build --serve
```

## Structure

- `00-MOCs/` — maps of content, the navigation hubs
- `01-Concepts/` — core concepts
- `02-Quick-References/` — cheatsheets
- `03-Projects/` — labs and practicals
- `04-Tools/` — tool notes
