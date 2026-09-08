# Contributing

This repo is **pre-buildable**: most of its planned content (compose bundles, Helm
chart, app-store listings — see [ROADMAP.md](ROADMAP.md)) is blocked on
[minderhq/minder#1253](https://github.com/minderhq/minder/issues/1253) (registry-based
image publishing) landing in the private core repo. Until that ships, there is no
published image for any manifest here to reference.

## What's useful to contribute right now

Not code — there's nothing to build against yet. The most useful thing you can do is
file a [New distribution target request](../../issues/new?template=new-distribution-target.yml)
describing:

- what self-host platform or format you'd want Minder packaged for (Umbrel, CasaOS,
  Runtipi, Coolify, Kubernetes/Helm, plain docker-compose, or something else), and
- why — your use case, and roughly how many people you think it would help.

That input directly shapes which of the Roadmap's phases gets prioritized once Phase 1
unblocks everything else.

## Once Phase 1 lands

Standard PR contribution flow will apply: branch, open a PR against `main`, and CI
(currently just markdown/YAML lint — see `.github/workflows/lint.yml`) must pass before
merge. This document will be expanded with real contribution guidelines (bundle
conventions, chart conventions, how to test a listing against its target platform) once
there's something concrete to contribute to.
