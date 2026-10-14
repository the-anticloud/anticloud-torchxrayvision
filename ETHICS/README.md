# Ethics — TORCHXRAYVISION

**Project:** TORCHXRAYVISION  
**Category:** MEDICAL_HEALTH  
**Upstream:** https://github.com/mlmed/torchxrayvision  
**Pinned commit:** `bd41a6ce24c4f69fd6e7fd6c8b263166a6bbd737`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `c16332b925c64650567bdda5d8477ab1f0fcaa17cc553cf77ead864f7ec6c490`  
**Date:** October 2026

## Position

TORCHXRAYVISION is packaged for offline deployment with a verifiable audit trail. The
ethical questions this raises are answered by making the system's behaviour
checkable rather than by policy statements.

## The four commitments

1. **No hidden egress.** The deployment has no external API dependency; this is
   testable by running it with the network disconnected.
2. **Attributable output.** Every artifact is recorded in a hash chain, so what
   the system produced can be reconstructed.
3. **Operator control.** The institution owns the hardware and the keys.
4. **Refusal to overclaim.** Where a certification is not held, the project says
   so rather than implying it.

## Dual use

This project is packaged for civilian and public-sector deployment. Where an
upstream has dual-use characteristics, the licence gate and the reference-only
marking in `BENCH.json` record that.
