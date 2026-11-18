<!-- SPDX-License-Identifier: Apache-2.0 -->
# Repository agent and merge guidance

## Priority 0 — additive superset, best-proven algorithms and full-history checkpoints (2026-11-18)

All work on this fork must preserve the **backward-compatible optimized superset** of applicable previously admitted upstream and fork capabilities, stable public API/ABI, behavior, tests and proven fixes/optimizations. Select the best *validated* candidate after comparing incumbent implementations and old/new branch and PR variants for correctness, speed, retained allocation and memory, portability, and maintainability. Add compatible APIs; never silently remove features, change contracts or discard good earlier code. Before every source promotion or merge, inspect earliest reachable ancestors plus relevant current/archived branches, tags, merged/closed/reverted PRs, merge bases and both merge parents for losses, bad commits and reintroduced regressions. Do not rebase `develop`. Record last checked date and exact commit SHA per ref, scan coverage/root, last verified anchor, findings and status; metadata-only or latest HEAD samples stay PENDING until fully examined. Canonical Synexia Maven/OpenRewrite recipes, provenance/history gates and repository-role boundaries govern source-changing work. New, separable first-party material prefers Apache-2.0; preserve all upstream/donor licenses, NOTICE and copyright. LLM-assisted atomization/patternization is not automatic independent authorship or relicensing. Canonical policy and PENDING 236-repo audit inventory: [com.synexia PR #18764](https://github.com/hsoliwal/com.synexia/pull/18764). Product-only forks must obey their existing clean postimage-only AGENTS boundaries.



## Superset merge and history-audit rule (2026-11-18)

Read [.github/MERGE_RULE.md](.github/MERGE_RULE.md) before changing an existing implementation or integrating any branch or PR.
Preserve the supported additive API/behavior superset and select implementations by correctness, predictability, measured performance, then elegance.
Audit all reachable history back to every root, all branches/tags and all PR states for lost code or regressions; preserve append-only history.
Record actual UTC coverage and remaining work in [.github/HISTORY_AUDIT.tsv](.github/HISTORY_AUDIT.tsv); policy installation is not a completed source-history audit.
Prefer verified Apache-2.0 sources and independent implementations while preserving all existing third-party rights; LLM rewriting does not remove license obligations.
Detailed private recipe/proof records remain in canonical Synexia; the compact local policy and checkpoint make the rule discoverable here.
<!-- MERGE-SUPERSET-HISTORY-20261118-2 -->
