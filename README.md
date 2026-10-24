# charly-internals

> **RETIRED.** This repo authors no skill any more. It is archived once its drain
> tag is minted; everything below is the destination map, kept so a reader who
> lands here knows where the prose went.

The `charly-internals` candy is a **concept candy**: it ships no install content
and carries no `skill:` node. All twenty `internals` skills have moved beside the
code, plugin, contract or config they describe, and are authored in those repos:

| Skill | Authored in |
|---|---|
| `capabilities`, `cloud-init-renderer`, `generate-source`, `libvirt-renderer`, `local-infra`, `ovmf`, `skills` | [opencharly/sdk](https://github.com/opencharly/sdk) |
| `disposable`, `install-plan` | [opencharly/spec](https://github.com/opencharly/spec) |
| `agents`, `cutover-policy`, `git-workflow`, `repo-setup`, `root-cause-analyzer` | [opencharly/opencharly](https://github.com/opencharly/opencharly) |
| `go`, `go-quality`, `plugin` | [opencharly/charly](https://github.com/opencharly/charly) |
| `egress` | [opencharly/plugin-egress](https://github.com/opencharly/plugin-egress) |
| `marketplace` | [opencharly/plugin-marketplace](https://github.com/opencharly/plugin-marketplace) |
| `vm-deploy-target` | [opencharly/plugin-deploy-vm](https://github.com/opencharly/plugin-deploy-vm) |

Their address is unchanged — every one still resolves as
`/charly-internals:<name>` — because `candy/plugin-marketplace` regenerates the
standalone [opencharly/marketplace](https://github.com/opencharly/marketplace)
corpus from each authoring repo's own body.

`git-workflow` and `repo-setup` are the two whose described config lives in
[opencharly/.github](https://github.com/opencharly/.github) — the org ruleset and
the required `charly-pr-validator` workflow. That repo holds neither a
`charly.yml` nor an `.agents/` tree, so it can be neither a skill host nor a
corpus source, and the umbrella is the only place a consumer can reach them from.
That is the deliberate exception in the umbrella's `AGENTS.md` Part II #5,
recorded in
[opencharly/opencharly#611](https://github.com/opencharly/opencharly/issues/611).

`sdk`, `spec`, the umbrella, `charly` and the three plugin repos are corpus
sources, so their eighteen skills publish from those repos today. The two this
repo drained last publish from the umbrella **once the refs leg lands**: the
refs list still pins this repo at `v2026.297.0301`, whose tree holds both, and
that ref is dropped in the same PR that advances the umbrella pin to a tag
carrying them.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `charly-internals` (concept candy) |
| Install content | none — a `true` no-op `plan:` |
| Authors | no skill — retired (the map above says where each one went) |
| Projected to | nothing; the corpus is generated from the authoring repos |
| Service / port | none |

The rest of the `internals` family is owned by sibling repos:
`layer-charly-internals-extra` (`strict-policy`, `vm-spec`, and the six agent
definitions) and `plugin-nerdctl` (`plugin-nerdctl`).

## What is still outstanding

Nothing in this repo. One leg remains in another repo: the
[opencharly/marketplace](https://github.com/opencharly/marketplace) refs list must
drop this repo's pin and advance the umbrella's, so the corpus reads
`git-workflow` and `repo-setup` from the umbrella instead of from this repo's
`v2026.297.0301` tag.

## How to use it

Do not consume this repo. Read a skill from the repo that authors it (the table
above), and load it as `/charly-internals:<name>` from the generated corpus.

## Layout

- `charly.yml` — the `charly-internals:` concept candy entity. It carries no
  `skill:` node.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skills: `/charly-internals:skills` (authored in `opencharly/sdk`),
  `/charly-internals:git-workflow` and `/charly-internals:repo-setup` (authored in
  the umbrella), `/charly-internals:marketplace` (authored in
  `opencharly/plugin-marketplace`)
- Authoring reference: `/charly-image:layer`
- Overflow skills: `opencharly/layer-charly-internals-extra`
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
