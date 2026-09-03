# hubblo-org/scaphandre context
> refreshed 2026-09-03 | upstream default: main @ 7155633 (dev) | PRs target dev

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

## Mined gaps (discovered, not yet attempted)
- `2026-09-03` process metric metadata fix (above) — status: attempted
