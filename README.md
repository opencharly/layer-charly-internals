# charly-internals

The `charly-internals` candy is a **concept candy**: it ships no install content.
It authors only the `internals` skills that still need to be authored here —
five of them:

`agents`, `cutover-policy`, `git-workflow`, `repo-setup`, and
`root-cause-analyzer`.

The other fifteen skills of the family have moved beside the code, plugin or
contract they describe, and are authored in those repos:

| Skill | Authored in |
|---|---|
| `capabilities`, `cloud-init-renderer`, `generate-source`, `libvirt-renderer`, `local-infra`, `ovmf`, `skills` | [opencharly/sdk](https://github.com/opencharly/sdk) |
| `disposable`, `install-plan` | [opencharly/spec](https://github.com/opencharly/spec) |
| `go`, `go-quality`, `plugin` | [opencharly/charly](https://github.com/opencharly/charly) |
| `egress` | [opencharly/plugin-egress](https://github.com/opencharly/plugin-egress) |
| `marketplace` | [opencharly/plugin-marketplace](https://github.com/opencharly/plugin-marketplace) |
| `vm-deploy-target` | [opencharly/plugin-deploy-vm](https://github.com/opencharly/plugin-deploy-vm) |

Their address is unchanged — every one still resolves as
`/charly-internals:<name>` — because `candy/plugin-marketplace` regenerates the
standalone [opencharly/marketplace](https://github.com/opencharly/marketplace)
corpus from each authoring repo's own body.

`sdk` and `spec` are already corpus sources, so their nine skills publish from
those repos today. The three plugin repos and `charly` are **not** sources yet,
so the six skills of `charly`, `plugin-egress`, `plugin-marketplace` and
`plugin-deploy-vm` are still SERVED from this repo's pinned tag until the corpus
refs list pins those four and advances this one; nothing a harness loads changes
in the move itself.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `charly-internals` (concept candy) |
| Install content | none — a `true` no-op `plan:` |
| Authors | five `internals` skills |
| Projected to | `marketplace/internals/skills/` |
| Service / port | none |

The rest of the `internals` family is owned by sibling repos:
`layer-charly-internals-extra` (`strict-policy`, `vm-spec`, and the six agent
definitions) and `plugin-nerdctl` (`plugin-nerdctl`).

## What is still outstanding

Three of the five already have their body in
[the umbrella](https://github.com/opencharly/opencharly) — `agents`,
`cutover-policy`, `root-cause-analyzer` — but the umbrella is not yet a source
in the corpus refs list, so this repo is still where the published corpus reads
them from. They move once the umbrella is pinned there.

`git-workflow` and `repo-setup` are filed as not routable
(opencharly/opencharly#611): their config home is `opencharly/.github`, which is
neither a submodule of the umbrella nor a corpus source.

The six skills that used to be outstanding here — `egress` (`plugin-egress`),
`go`, `go-quality` and `plugin` (`charly`), `marketplace` (`plugin-marketplace`),
`vm-deploy-target` (`plugin-deploy-vm`) — are DRAINED: each destination authors
its copy, byte-identically. What remains for them is the corpus refs leg: pin
those four repos in `marketplace/candy/charly-marketplace/charly.yml` and advance
this repo's pin, so the published corpus reads them from their new homes instead
of this repo's tag.

## How to use it

This repo is consumed as a **skill source**, not as an image layer. Edit a
skill's prose in the repo that authors it (the table above), then regenerate the
corpus. To reference the repo directly, compose it in a box. A box is a
`candy:` node that carries the box's `base:` image and a nested `candy:` list of
layer refs (the nested `candy:` is the composition list; the outer `candy:` is
the box body):

```yaml
my-box:
  candy:                  # the box body (an IMAGE is a `candy:` node carrying `base:`)
    base: fedora          # the box's base image
    candy:                # the box's composition list
      - '@github.com/opencharly/layer-charly-internals:v2026.271.1802'
```

## Layout

- `charly.yml` — the `charly-internals:` concept candy entity plus the routing
  metadata for the five skills it still authors (`agents`, `cutover-policy`,
  `git-workflow`, `repo-setup`, `root-cause-analyzer`).
- `.agents/skills/` — the bodies of those five skills.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skills: `/charly-internals:skills` (authored in `opencharly/sdk`),
  `/charly-internals:git-workflow`, `/charly-internals:marketplace`
- Authoring reference: `/charly-image:layer`
- Overflow skills: `opencharly/layer-charly-internals-extra`
- [`opencharly/marketplace`](https://github.com/opencharly/marketplace) — the projected corpus
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
