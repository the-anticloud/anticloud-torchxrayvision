# Educators — TORCHXRAYVISION

**Project:** TORCHXRAYVISION  
**Category:** MEDICAL_HEALTH  
**Upstream:** https://github.com/mlmed/torchxrayvision  
**Pinned commit:** `bd41a6ce24c4f69fd6e7fd6c8b263166a6bbd737`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `c16332b925c64650567bdda5d8477ab1f0fcaa17cc553cf77ead864f7ec6c490`  
**Date:** October 2026

## Teaching with TORCHXRAYVISION

The project is usable as a worked example of offline-first packaging with a
cryptographic audit chain. It ships with the assurance suite, the evidence
register and the ledger, so students can verify claims rather than take them on
faith.

## Suggested exercises

1. Run `python tools/run_bench.py --out BENCH.json` and read the 16 results.
2. Recompute the SHA3-256 of a row's evidence file and compare to the register.
3. Walk the AIOSS chain from genesis to head `c16332b925c64650567bdda5d8477ab1f0fcaa17cc553cf77ead864f7ec6c490` and confirm every link.
4. Break one artifact and observe the chain fail to verify.

## Licence for teaching

Apache 2.0 terms apply to academic and teaching use. See `24_ANTICOMMONS_LICENSE`.
