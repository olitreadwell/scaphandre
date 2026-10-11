# hubblo-org/scaphandre context
> refreshed 2026-10-11 | upstream main @ 04cc6ec | PR base branch: dev @ 7155633

## Identity & policies
- upstream: hubblo-org/scaphandre, default branch main, primary language Rust, English-first (yes — docs/contributing in English)
- CLA/DCO: none (no CLA bot, no DCO requirement found in CONTRIBUTING)
- AI-assisted PR policy: unstated (no ban found)
- signed commits required: no (no required_signatures branch protection)
- PR template: none (no .github/PULL_REQUEST_TEMPLATE.md) — use pipeline pr-body-template.md fallback
- external tracker: github

## Conventions (verified from merged PRs)
- PRs must target `dev` (docs_src/contributing.md + CI check `check_pr_is_on_the_right_branch` fails if base=main and head != dev)
- branch naming: mixed (`fix/...`, `docs/...`, `feat-...`, `patch-1`); `fix/` is the dominant human pattern
- commit style: Conventional Commits (`fix:`, `feat:`, `style:`, `docs:`)
- CI: `fmt_and_clippy_linux` (ubuntu-latest, `cargo fmt --all -- --check` + `cargo clippy -- -A clippy::upper_case_acronyms -D warnings`); test/build jobs run on AWX bare-metal workers needing upstream secrets
- CI trigger note: the `Tests` workflow `paths-ignore`s `docs_src/**` and `*.md`, so a documentation-only PR produces NO Actions run at all (not a red PR — simply nothing is triggered)
- how outside PRs get merged: responsive; external merges seen (fix/cgroups-v2-container-detection, correct-typo, riemann_fix)

## Maintainer picture
- active maintainer: bdromard (Benoit Petit); responsive to PRs
- areas in flight: cgroups v2 container detection, qemu exporter, prometheus push mode

## Issue-area health
- open issues with maintainer engagement are rare; most open issues are questions or unassigned
- #428 (qemu virtiofs) claimed by open PR #429; #434 (prometheus freeze) claimed by open PR #435
- #420 (cgroups v2) already fixed by merged PR #423

## Gap ledger (dedupe — READ FIRST, never re-pick)
- `2026-09-03` self-found gap: process metric metadata errors in `src/sensors/mod.rs` `get_all_per_process()` — `scaph_process_memory_virtual_bytes` uses `Unit::Percentage` (should be `Bytes`); `scaph_process_power_consumption_microwatts` description is a copy-paste of the disk-read text. Verified present on `origin/dev`. No upstream issue/PR references these strings. outcome: pr-opened — lesson: copy-paste metadata bugs in metric definitions are real, verifiable, low-risk fixes.
- `2026-09-30` self-found gap: broken documentation links, a dead external link and a wrong sensor reference — `explanations/host_metrics.md` links `explanations/rapl-domains.md` (resolves to a non-existent `explanations/explanations/rapl-domains`), `references/exporter-stdout.md` links `references/metrics.md`, the Grafana how-to anchors to `exporter-prometheus.md#scaph_process_power_consumption_microwatts` (no such heading on that page), `why.md` links a 404 IEA report, and `sensor-msr_rapl.md` tells users to call the `powercap_rapl` sensor. Verified in the mdbook-built site (11 broken internal targets before, 3 pre-existing after) and with direct HTTP requests (old IEA URL 404, replacement 200). Dedupe: open upstream PR #426 "fix: spelling" (weriomat, base main, 21 files) covers only spelling typos (succeds, detailled, informations, explicitely, Celcius, ...), so every fix in this pass is deliberately distinct from it; #288 "Some links in the documentation seem to be broken" is closed (its own links fixed by #259). outcome: pr-opened — lesson: for a spelling-heavy repo, check the open spelling PR first and trim typos it already covers; doc-only PRs never trigger Actions.
- `2026-10-11` self-found gap: stale commands, wrong paths and stray typos in the docs — the JSON exporter reference documented `-n` for the nano-second step (only `--step-nano` exists; `src/exporters/json.rs` declares it `long`-only), `how-scaph-computes-per-process-power-consumption.md` pointed at `/proc/stats` and `/proc/PID/stats` (real paths `/proc/stat` and `/proc/PID/stat`; `KernelStats` reads `/proc/stat`), `installation-linux.md` pinned `VERSION="1.0.1"` whose `.deb` download 404s (no v1.0.1 release; v1.0.2 ships deb11/deb12, verified via the releases API), and the Warp10 / Prometheus-push references quoted defaults that no longer match the code (warp10 send interval 60s → 2s; prometheus-push `--step` 5 → 30). Plus one-word typos: `the the`, `and so one`, `a machines'`, `scrapped`, `Comsumption`, `stracktrace`, `form the one below`, `not item`. Dedupe: upstream PR #426 covers only its own spelling list (not these lines) and fork PR #2 covers the broken links/`Suming`/`it's own`/`Therefor`/`powercap_rapl`→`msr_rapl` — none overlap. outcome: pr-opened — lesson: the doc references are a rich source of stale CLI flags/defaults; always diff every quoted `-h` block and command example against `src/exporters/*.rs`.

## Mined gaps (discovered, not yet attempted)
- `2026-09-03` process metric metadata fix (above) — status: attempted
- `2026-09-30` broken doc links / dead link / wrong sensor name (above) — status: attempted
- `2026-10-11` stale doc commands / paths / defaults + stray typos (above) — status: attempted
