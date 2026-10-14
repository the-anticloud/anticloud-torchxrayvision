# Students — TORCHXRAYVISION

**Project:** TORCHXRAYVISION  
**Category:** MEDICAL_HEALTH  
**Upstream:** https://github.com/mlmed/torchxrayvision  
**Pinned commit:** `bd41a6ce24c4f69fd6e7fd6c8b263166a6bbd737`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `c16332b925c64650567bdda5d8477ab1f0fcaa17cc553cf77ead864f7ec6c490`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `bd41a6ce24c4f69fd6e7fd6c8b263166a6bbd737`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `c16332b925c64650567bdda5d8477ab1f0fcaa17cc553cf77ead864f7ec6c490`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
