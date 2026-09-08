# deploy

Deployment & distribution artifacts for **Minder**: docker-compose bundles, a Helm chart, and self-hosted app-store listings (Umbrel / CasaOS / Runtipi / Coolify).

Status: **planned** — tracked in [minderhq/minder#1259](https://github.com/minderhq/minder/issues/1259).

> Note: under the closed-core model these manifests reference the **private** core images, so self-hosting is **licensed** (not open community self-host).

## Roadmap

This repo is scaffolded ahead of most of its content being buildable. See
[ROADMAP.md](ROADMAP.md) for the full plan; tracked upstream in
[minderhq/minder#1259](https://github.com/minderhq/minder/issues/1259) and
[minderhq/minder#1253](https://github.com/minderhq/minder/issues/1253).

- **Phase 1 — Compose bundles** (`compose-bundles/`): blocked on
  [minder#1253](https://github.com/minderhq/minder/issues/1253) (registry-based image
  builds). Once first-party images are published to a registry, this directory will hold
  ready-to-run `docker-compose.yml` files that reference those images directly — no
  source build required, unlike `minder`'s own dev-oriented compose file.
- **Phase 2 — Helm chart** (`helm/`): a chart for Kubernetes-based self-hosting, for
  operators beyond a single-box deployment.
- **Phase 3 — App-store listings** (`app-store-listings/`): one-click install listings
  for Umbrel, CasaOS, Runtipi, and Coolify, each in their own app-store's manifest
  format.
