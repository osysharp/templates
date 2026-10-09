# Harbour's help articles

The public help centre's articles, one markdown file per article and language:

- `en/<slug>.md` is the original. Its front-matter says where it sits: `category`, `section` (both slugs from
  `shelves.json`), `audience` (always `Everyone` here), `position` in its section, and `version`.
- `sv/<slug>.md` and `ar/<slug>.md` are its Swedish and Arabic versions: the same `slug`, their own `title`, `summary`
  and `version`. Commands and code stay exactly as in English.
- `shelves.json` holds the categories and sections, with their names in all three languages.

## After editing

```console
python3 content/help/build.py           # writes data/help/*.json and the fixture block in tests/help-articles.test.osy
python3 content/help/build.py --check   # exit 1 if either is behind the markdown
osy test --file tests/help-articles.test.osy
```

## Loading them into a desk

The articles are data, so they travel with `osy import` (the manifest's `data "data/**/*.json"` glob picks up
`data/help/`). An import is an ordinary write, so it runs as one of the desk's own accounts:

- **Categories and sections** need `ManagesKnowledge` (an admin). Harbour's admins sign in with a second step, which
  `--as` cannot give (measured 2026-10-05: *"that account signs in with a second step … which `--as` cannot give"*).
  So an admin makes the four categories and their sections on **Knowledge base › Categories**, with the slugs and the
  three languages' names in `shelves.json`. A later import finds them by slug and leaves them alone.
- **Articles and their versions** need `WritesKnowledge`: import as a teammate (an Agent), whose account has no second
  step.

```console
osy import --as <agent-email>                                         # the local desk
osyrin app import --app <slug> --server <platform> --as <agent-email>   # a deployed desk
```

An import writes every version as a **draft**; it never publishes. Then, on the desk, send each draft for review
(Knowledge base) and have a lead publish it (Knowledge base › Waiting for review), exactly as for an article written
there. Re-running the import is safe: rows are matched by their slug (a version by article, language and number), and
what is unchanged is left alone.

To change an article that is already published, raise `version:` in that language's file (2, 3, …), rebuild and
import: the new version arrives as a draft beside the published one, and publishing it replaces it. A published
version's words are never changed by an import.
