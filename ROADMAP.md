# Roadmap

`deploy` holds the deployment and distribution artifacts for Minder: docker-compose
bundles, a Helm chart, and self-hosted app-store listings. Almost all of it is blocked on
one prerequisite upstream in the private core repo — this document lays out the phases
so the plan is visible even while most of the work waits.

Tracked upstream:

- [minderhq/minder#1259](https://github.com/minderhq/minder/issues/1259) — distribution
  epic (app-store listings + Helm chart).
- [minderhq/minder#1253](https://github.com/minderhq/minder/issues/1253) — move
  first-party images to a registry (build-push in CI, pull-on-deploy). This is the hard
  gate: app stores and compose bundles pull published images, they don't build from
  source.

## Phase 1 — Compose bundles

**Delivers:** ready-to-run `docker-compose.yml` bundle(s) under `compose-bundles/` that
a self-hoster can `docker compose up` against without cloning the private core repo or
building anything locally.

**Blocked on:** [minder#1253](https://github.com/minderhq/minder/issues/1253). Today,
Minder's own compose file builds all first-party service images from source
(`context: ../` + `dockerfile: src/services/*/Dockerfile`) — there is no published image
to reference yet. Once CI in the core repo builds and pushes `minder/<name>:<tag>` to a
registry (Harbor or GHCR), a compose file can reference those images directly.

**Rough shape once unblocked:** something like
`compose-bundles/standard/docker-compose.yml` with no `build:`/`context:` sections —
only `image: harbor.example/minderhq/api-gateway:<tag>` (or the GHCR equivalent)
references, one per service, plus whatever `.env.example` and volume/network
declarations a standalone bundle needs. The exact registry, tagging scheme, and bundle
variants (if more than one) aren't decided yet — that detail lands with #1253, not here.

## Phase 2 — Helm chart

**Delivers:** a Helm chart under `helm/` for Kubernetes-based self-hosting, for
operators who want more than a single-box deployment.

**Blocked on:** the same registry prerequisite as Phase 1 (a Helm chart's
`values.yaml` needs real image references to point at), plus Phase 1 landing first as
the reference for which services/ports/env vars need to be modeled as Kubernetes
resources.

**Rough shape once unblocked:** a standard chart layout
(`helm/minder/Chart.yaml`, `helm/minder/values.yaml`, `helm/minder/templates/*.yaml`)
with one Deployment + Service per first-party image, mirroring the compose bundle's
service list and env vars rather than inventing a separate topology.

## Phase 3 — App-store listings

**Delivers:** one-click install listings under `app-store-listings/` for Umbrel,
CasaOS, Runtipi, and Coolify — each platform's own app-store manifest format, pointing
at the same published images as Phase 1.

**Blocked on:** Phase 1 (each of these formats is, at its core, a thin manifest wrapping
a compose-like service list — there's nothing to wrap until Phase 1's images and
bundle exist).

**Rough shape once unblocked:** one subdirectory per platform (`app-store-listings/umbrel/`,
`casaos/`, `runtipi/`, `coolify/`), each holding that platform's required manifest
file(s) (e.g. Umbrel's `umbrel-app-store.yml` + `docker-compose.yml` pair, CasaOS's
`docker-compose.yml` + config, Runtipi's app config, Coolify's service definition) —
the exact per-platform schema isn't written up here since it's dictated by each
platform, not by this repo.

## How to help

Since Phases 1–3 all wait on [minder#1253](https://github.com/minderhq/minder/issues/1253),
the most useful contribution right now isn't code — there's nothing to build against
yet. If you'd like a specific platform or format supported, or have requirements for
one already listed above, please file a
[New distribution target request](../../issues/new?template=new-distribution-target.yml)
issue. See [CONTRIBUTING.md](CONTRIBUTING.md) for more.
