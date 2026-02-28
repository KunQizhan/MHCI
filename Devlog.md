# Development Log

1. Initial version: marking menu renders at the exact finger-down position.
2. Testing on phone near screen edges: menu sectors overflow off-screen, labels invisible.
3. Added screen-edge clamping — menu center offsets inward when too close to edges.
4. New problem: after clamp, the visual menu center no longer matches the angle computation origin (`touchStart`), causing the highlighted sector to disagree with the actual selected sector. Misselection rate noticeably higher near edges.
5. Fixed by switching angle computation to use the rendered `menuCenter` instead of raw `touchStart` once the menu is visible. Verified by testing in all four screen corners.
6. Baseline button layout changed from 3×3 (with 2 orphan buttons on the last row) to 4×2 symmetric grid, to reduce positional bias across the 8 targets.
7. Removed `hysteresisAngle` config that was declared but never used in the sector-switching logic. Instead, `sectorCrossings` is logged per trial as a proxy metric for boundary instability — plan to discuss in future work.
8. Cleaned up CSV export: originally filtered out all `gesture*` fields then added summary fields back with Set dedup — simplified to just exclude `gesturePath` (the raw coordinate array) and keep summary fields naturally.
9. Removed `sectorCenter()` (defined but never called) and `lastCommittedSector` (initialised but never read). Less dead code = fewer questions from markers.
10. Added Web Audio feedback (no audio files): sector-change tick (throttled 60ms), confirm click, error click. Compensates for Vibration API not working on iOS — ensures consistent cross-device feedback.
11. Reduced trial load: practiceTrials 8→4, formalTrials 24→16 (8 targets × 2 reps). Total per participant drops from 96 to 60 task trials.
12. Removed NASA-TLX from per-condition questionnaire to reduce participant burden. Kept SUS (10 items) + 3 custom Likert items per condition (13 items × 3 = 39 total, down from 57). NASA-TLX Performance item had confusing Good/Poor label direction — removing TLX also resolved this.
13. Added Firebase Realtime Database integration: auto-upload on study completion, admin dashboard (admin.html) for monitoring and CSV/JSON download. Manual download buttons kept as fallback.
14. Cleaned up CSV export: just exclude gesturePath array, keep all summary fields. Removed unused sectorCenter() and lastCommittedSector.
15. Final state: prototype ready for deployment. Trial flow: 4 practice + 16 formal per condition × 3 conditions = 60 task trials + 39 questionnaire items. ~10–15 min per participant.