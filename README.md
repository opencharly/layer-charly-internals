# charly-internals

The `charly-internals` candy is a **concept candy**: it ships no install content.
It authors only the `internals` skills that still need to be authored here —
two of them:

`git-workflow` and `repo-setup`.

The other eighteen skills of the family have moved beside the code, plugin or
contract they describe, and are authored in those repos:

| Skill | Authored in |
|---|---|
| `capabilities`, `cloud-init-renderer`, `generate-source`, `libvirt-renderer`, `local-infra`, `ovmf`, `skills` | [opencharly/sdk](https://github.com/opencharly/sdk) |
| `disposable`, `install-plan` | [opencharly/spec](https://github.com/opencharly/spec) |
| `agents`, `cutover-policy`, `root-cause-analyzer` | [opencharly/opencharly](https://github.com/opencharly/opencharly) |
| `go`, `go-quality`, `plugin` | [opencharly/charly](https://github.com/opencharly/charly) |
| `egress` | [opencharly/plugin-egress](https://github.com/opencharly/plugin-egress) |
| `marketplace` | [opencharly/plugin-marketplace](https://github.com/opencharly/plugin-marketplace) |
| `vm-deploy-target` | [opencharly/plugin-deploy-vm](https://github.com/opencharly/plugin-deploy-vm) |

Their address is unchanged — every one still resolves as
`/charly-internals:<name>` — because `candy/plugin-marketplace` regenerates the
standalone [opencharly/marketplace](https://github.com/opencharly/marketplace)
corpus from each authoring repo's own body.

`sdk`, `spec` and the umbrella are corpus sources, so their twelve skills publish
from those repos today — the umbrella's three (`agents`, `cutover-policy`,
`root-cause-analyzer`) since
[marketplace#567](https://github.com/opencharly/marketplace/pull/567) pinned it
at `v2026.296.2254`. The three plugin repos and `charly` are **not** sources yet,
so the six skills of `charly`, `plugin-egress`, `plugin-marketplace` and
`plugin-deploy-vm` are still SERVED from this repo's pinned tag until the corpus
refs list pins those four; nothing a harness loads changes in a move itself.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `charly-internals` (concept candy) |
| Install content | none — a `true` no-op `plan:` |
| Authors | two `internals` skills |
| Projected to | `marketplace/internals/skills/` |
| Service / port | none |

The rest of the `internals` family is owned by sibling repos:
`layer-charly-internals-extra` (`strict-policy`, `vm-spec`, and the six agent
definitions) and `plugin-nerdctl` (`plugin-nerdctl`).

## What is still outstanding

`git-workflow` and `repo-setup` are filed as not routable
([opencharly/opencharly#611](https://github.com/opencharly/opencharly/issues/611)):
their config home is `opencharly/.github`, which is neither a submodule of the
umbrella nor a corpus source. They are the reason this repo cannot be archived
yet.

The three skills that used to be authored here as a second copy — `agents`,
`cutover-policy`, `root-cause-analyzer` — are DRAINED: the umbrella authors them
and the corpus publishes them from there.

The six skills that used to be outstanding here — `egress` (`plugin-egress`),
`go`, `go-quality` and `plugin` (`charly`), `marketplace` (`plugin-marketplace`),
`vm-deploy-target` (`plugin-deploy-vm`) — are DRAINED too: each destination
authors its copy. What remains for them is the corpus refs leg: pin those four
repos in `marketplace/candy/charly-marketplace/charly.yml`, so the published
corpus reads them from their new homes instead of this repo's tag. **This repo's
pin cannot advance before that leg lands** — it is the only corpus source those
six have, and `charly` cannot be pinned at all while
`sdk/candywalk`'s `acceptedRef` refuses `github.com/opencharly/charly` by design.

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
  metadata for the two skills it still authors (`git-workflow`, `repo-setup`).
- `.agents/skills/` — the bodies of those two skills.
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
