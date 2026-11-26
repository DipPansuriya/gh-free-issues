# gh-free-issues

A [GitHub CLI](https://cli.github.com/) extension that finds open issues nobody is already working on.

Labels like `good first issue` or `accepting prs` don't tell you whether someone already opened a pull request or asked to take the issue. On popular repos, most labeled issues are already taken. `gh free-issues` checks that for you.

An issue is **taken** if it has:

- an assignee,
- an open pull request in the same repo that references it, or
- a recent comment claiming it ("can I work on this?", "I'll take this", ...).

Earlier closed PRs are shown as a hint, since they often contain useful discussion.

## Install

```sh
gh extension install DipPansuriya/gh-free-issues
```

Requires only `gh` (logged in with `gh auth login`) and Bash.

## Usage

```sh
gh free-issues typescript-eslint/typescript-eslint -l "accepting prs"
```

```text
Free issues in typescript-eslint/typescript-eslint (4 of 25 checked)

#13147  Bug: Parser should throw an error for method shorthands in assignment object destructuring patterns
       3 comments, opened 2026-11-05, earlier closed PR #13168 · https://github.com/typescript-eslint/typescript-eslint/issues/13147
...
```

Show taken issues and the reason:

```sh
gh free-issues typescript-eslint/typescript-eslint -l "accepting prs" --all
```

```text
Taken (21)

#13092  Docs: rule "restrict-template-expressions" is missing in TypeOrValueSpecifier's ...
       open PR #13171, #13225
#13089  Bug: [no-unnecessary-type-assertion] autofix changes inferred return type ...
       open PR #13082
```

Run inside a cloned repo to omit the repository argument.

### Flags

| Flag | Description |
| --- | --- |
| `-l, --label <name>` | Only issues with this label (repeatable; all must match) |
| `-L, --limit <n>` | Number of newest issues to inspect, max 100 (default 50) |
| `-d, --claim-days <n>` | Ignore claim comments older than n days (default 30) |
| `-a, --all` | Also show taken issues and why |
| `--json` | Output JSON (free issues only, or all with `--all`) |
| `--no-color` | Disable colored output. Colors are also off when `NO_COLOR` is set or output isn't a terminal. |

## How it works

One GraphQL search request fetches the issues along with their assignees, latest comments and cross-referenced pull requests, so it's fast even for 100 issues.

## Limitations

- Claim detection is a heuristic based on common phrases and only looks at the last 30 comments.
- Only the last 50 cross-references per issue are checked.

## License

MIT
