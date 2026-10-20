---
name: release-consumers
description:
  Which sibling repos require a go-gui version bump on release. Use when cutting
  a go-gui release, propagating a new tag to consumers, or deciding whether a
  repo is an upstream or downstream dependency.
---

# go-gui release consumers

- Direct go-gui consumers (require a go-gui bump on release): **go-charts,
  go-edit, go-kite, go-map, go-speedtest, go-term**. Verified against each
  repo's `go.mod` `require` (all `github.com/go-gui-org/*`). go-glyph is
  _upstream_ — go-gui depends on it, not the reverse; never a bump target.
- On release, re-verify the list from `go.mod` files; don't rely on memory. New
  consumers get added without updating this note.

## Release order

1. Tag go-glyph.
2. Tag go-gui.
3. Bump and tag the tier-1 repos (they require only glyph/gui): go-charts,
   go-edit, go-kite, go-map, go-term.
4. Bump the tier-2 repos: go-speedtest. It requires go-charts and go-map, so it
   resolves their **published tags**. A merged tier-1 PR is not visible to it
   until that repo is tagged.

The global `sync-siblings` skill runs this order.

## Per-repo gotchas

- **go-charts** CI checks out go-gui and go-glyph by tag. A release bumps
  `go.mod` **and** the `ref:` pins in `.github/workflows/ci.yml` and
  `.github/workflows/gallery.yml`. The other repos need only `go.mod`/`go.sum`.
- **go-speedtest** keeps the `go` directive in `go.mod` at 1.26.x. The released
  golangci-lint binary is built with go1.26 and refuses a higher target.
