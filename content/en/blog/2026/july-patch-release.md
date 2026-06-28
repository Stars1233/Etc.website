---
title: July Patch Release: v3.5.32 and v3.6.13
author:  "SIG-Etcd Leads"
date: 2026-07-01
draft: false
---

SIG-etcd has released routine patch updates for the v3.5 and v3.6 release branches.  These releases address dependency CVEs, fix a websocket authentication bug, and add a new option to help operators complete the v2 storage deprecation.  Users on v3.5 and v3.6 should update at the next scheduled maintenance window.

Obtain the updates here:

* [v3.6.13](https://github.com/etcd-io/etcd/releases/tag/v3.6.13)
* [v3.5.32](https://github.com/etcd-io/etcd/releases/tag/v3.5.32)

Official container images are available from [gcr.io](https://gcr.io/etcd-development/etcd).

As noted in the [June release](https://etcd.io/blog/2026/june-patch-release/), v3.4 reached end of support and will not receive any further patches.  If you are still running v3.4, please [upgrade to a supported version](https://etcd.io/docs/v3.6/upgrades/upgrade_3_5/) as soon as possible.

## Patching dependency CVEs

This release updates v3.5 and v3.6 to [golang v1.25.11](https://groups.google.com/g/golang-nuts), and bump `go.opentelemetry.io/otel` and `go.opentelemetry.io/otel/sdk` from `v1.40.0` to `v1.43.0` to address [CVE-2026-29181](https://github.com/advisories) and [CVE-2026-39883](https://github.com/advisories).  v3.6.13 additionally bumps `golang.org/x/crypto` to `v0.52.0` to resolve several CVEs.

It is unknown how many of these vulnerabilities are exploitable in etcd, but users should plan to apply the patch as soon as convenient regardless.

If you find a vulnerability in etcd, please report it to [our security team](mailto:security@etcd.io).

## Fixing websocket authentication with bearer-prefixed tokens

Both releases include a fix for [websocket authentication with bearer-prefixed auth tokens](https://github.com/etcd-io/etcd/pull/21932), which previously caused authenticated websocket requests to be rejected when the token included the `Bearer` prefix.

## New `write-only-skip-check` option for `--v2-deprecation`

To help operators finish migrating off of the long-deprecated v2 storage, both releases add a new [`write-only-skip-check` option for `--v2-deprecation`](https://github.com/etcd-io/etcd/pull/21850) that bypasses the v2 content check during startup.  This is useful for clusters that have already migrated their data but still carry residual v2 records.

In addition, v3.5.32 [enhances `etcdutl check v2store`](https://github.com/etcd-io/etcd/pull/21889) to check both v2 snapshot and WAL records, giving operators a more complete picture of any remaining v2 data before turning off v2 support entirely.

## Other improvements in v3.5.32

v3.5.32 backports the [`server: allow non-admin maintenance status`](https://github.com/etcd-io/etcd/pull/21811) change that previously shipped in v3.6.12, allowing non-admin users to call the maintenance `Status` endpoint.

This release also includes additional reliability fixes, which can be found in the full changelogs:

* [CHANGELOG-3.6](https://github.com/etcd-io/etcd/blob/main/CHANGELOG/CHANGELOG-3.6.md#v3613)
* [CHANGELOG-3.5](https://github.com/etcd-io/etcd/blob/main/CHANGELOG/CHANGELOG-3.5.md#v3532)
