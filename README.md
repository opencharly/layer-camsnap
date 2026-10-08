# camsnap

RTSP/ONVIF camera snapshot and clip CLI for OpenCharly images.

The `camsnap` candy installs the upstream [camsnap](https://github.com/steipete/camsnap)
Go CLI — a snapshot and clip tool for RTSP/ONVIF cameras. It is a package-only
layer: a `go install` build step places the `camsnap` binary in the user's
GOPATH bin (`~/go/bin/camsnap`, on `$PATH`), and there is no service, daemon, or
configuration. It composes into any image that can run the Go toolchain, pulling
the `golang` runtime in as a dependency.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `camsnap` |
| Binary | `~/go/bin/camsnap` (on `$PATH`) |
| Build | `go install github.com/steipete/camsnap/cmd/camsnap@latest` |
| Depends | `layer-golang` |
| Environment | `GOPATH=~/go`; `PATH` append `~/go/bin` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-camsnap:v2026.243.0408'
```

Then, inside the built image:

```bash
camsnap --help
camsnap snapshot <camera-url> out.jpg
```

## Layout

- `charly.yml` — the candy manifest: the `camsnap:` candy entity (a `go install`
  `run:` step plus `check:` assertions) and the embedded `camsnap-skill:` entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:camsnap`
- Required parent: `/charly-coder:golang`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
