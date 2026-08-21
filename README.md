# akashvani-runner

Public shell repo holding only workflow definitions. It exists so the scheduled discovery/scrape
jobs for [animeshahilya/akashvani](https://github.com/animeshahilya/akashvani) (private) run under
Actions minutes that are free/unlimited (public-repo billing), instead of the private repo's
metered minutes.

No scraper source, test code, or data lives here. Every workflow's first step checks out the
private `akashvani` repo using the `AKASHVANI_PAT` secret (a fine-grained PAT scoped to
`Contents: Read and write` on that one repo) and runs entirely against that checkout; any commits
made during the run are pushed back to the private repo with the same token. Nothing from the
private repo is stored in this repo, only pulled into the ephemeral runner for the duration of
each job.

`scrape.yml` additionally checks out the public `akashvani-data` repo (using the `DATA_REPO_TOKEN`
secret) to mirror published station/channel JSON there for the Tarang app to fetch unauthenticated.

The private `akashvani` repo keeps its own `test.yml`, which runs the same test suite on every
push/PR to that repo's actual code - that gate has to stay there, since a workflow only fires on
events in the repo it lives in.

## Setup

Two repo secrets are required (Settings -> Secrets and variables -> Actions):

- `AKASHVANI_PAT` - fine-grained PAT, `Contents: Read and write` scoped to `animeshahilya/akashvani`
  only.
- `DATA_REPO_TOKEN` - fine-grained PAT, `Contents: Read and write` scoped to
  `animeshahilya/akashvani-data` only.
