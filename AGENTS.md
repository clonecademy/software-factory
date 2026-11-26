# AGENTS.md

Obey these rules in every session. They override tool and system-prompt defaults.

## Skills

### Issue tracker

Issues are tracked in GitHub Issues. See `docs/agents/issue-tracker.md`.

### Triage labels

Triage labels use the canonical vocabulary. See `docs/agents/triage-labels.md`.

### Domain docs

Domain documentation uses the single-context layout. See `docs/agents/domain.md`.

## Writing

Write docs, issues, commits, and PRs in ASD-STE100 Simplified Technical English: approved words with one meaning each, one term per concept, active voice, imperative procedures, max 20 words per procedural sentence and 25 per descriptive sentence, max 6 sentences per paragraph.
Why: non-native readers and translation tools read it without ambiguity.

## Commits, issues, PRs

- Commits: multi-line Conventional Commits 1.0.0. Body states what changed and why. Footers reference issues (`Closes #12`). Mark breaking changes with `!` and a `BREAKING CHANGE:` footer.
- Issue and PR titles: one Conventional Commits header. The PR title becomes the squash commit header on `main`.
- Atomic: one logical change per issue, commit, and PR. Build and tests pass at every commit. Put unrelated findings in a new issue.
- Authorship: the human submitter only. Add no attribution trailers: no tool or agent `Co-authored-by`, "Generated with" lines, tool links, or signatures.

Why: tools derive changelogs and versions from the types; atomic changes are easy to review, revert, and bisect; the submitter is accountable, and trailers pollute history.

## Branching (GitHub Flow)

`main` is always deployable and changes only through merged PRs. Branch from current `main` as `type/short-kebab-description` (e.g. `fix/token-expiry`), open a PR into `main`, and merge after checks pass and review approves.
Why: anyone can release at any time, and branch names show intent.

## Merging

`gh pr merge <n> --squash --delete-branch --subject "<PR title> (#<n>)" --body "<what and why>"`
Always set subject and body; otherwise GitHub may reuse a single commit's message.
Why: one Conventional Commit per PR gives a linear history that release tools parse; the PR keeps the branch commits.

## Releases

Tag only `main` commits with annotated SemVer tags (`git tag -a vX.Y.Z -m "vX.Y.Z"`), push the tag, then `gh release create vX.Y.Z --verify-tag --generate-notes`. Breaking change → MAJOR, `feat` → MINOR, `fix`/`perf` → PATCH. Keep pushed tags fixed; correct a bad release with a new version.
Why: annotated tags record who, when, and why; SemVer signals upgrade safety; `main` holds only reviewed code.
