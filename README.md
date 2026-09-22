# DocuSign Clone — QA Bug Hunt, Triage & Reporting

Comparative QA pass of a DocuSign clone against the original DocuSign app, focused on what matters most to users: pixel-accurate UI fidelity and functional flows that hold up end to end.

I tested side-by-side at a fixed viewport, logged every defect with blind-reproducible steps, triaged by severity, and attached video/image evidence for each bug.

## Environment

- **Browser:** Chrome (latest), 1280×960, 100% zoom
- **OS:** macOS
- **Reference:** DocuSign free trial account (source of truth)
- **Target:** DocuSign clone
- **Tools:** Chrome DevTools (spacing, computed font weight, hex colors, console), pixel-overlay extension, screenshot + screen recorder

All bugs in `bug_tracker_Athul.csv` are reproducible in this environment.

## Scope

**Tested:**
- Pixel-level UI issues: alignment, spacing, color, font weight, icon size, responsive breaks
- Functional flows: broken states, wrong navigation, form validation, error handling
- Edge cases: empty states, invalid inputs, duplicates, disabled/empty controls
- Hard-coded / stale data: placeholder text, static counters, dead controls
- Critical flows: anything that corrupts data or blocks sending/signing

**Deliberately excluded:**
- CSV / PDF / image export (not built)
- Third-party integrations (Slack, Gmail, Drive, Notion)
- Performance / load benchmarking (obvious freezes noted only)
- Security / penetration testing

## Methodology

1. Fixed environment first: Chrome at 1280×960, 100% zoom.
2. Opened original DocuSign and clone side by side, walked the happy path end to end without logging: login → dashboard → upload → add recipients → place fields → send → manage → sign.
3. Built a module checklist: auth/landing, dashboard, upload, recipients/routing, field placement, send/confirmation, manage/status, templates, global nav/modals/toasts.
4. Pixel pass per screen with overlay: spacing, font weight, colors, icon size, hover/focus/disabled/loading/empty states.
5. Functional pass per module: happy path, validation (required, invalid email, duplicates), navigation (back, refresh, direct URL), error handling.
6. Edge push: invalid/duplicate emails, single-recipient signing order, disabled options, dead links/icons.
7. Logged one row per bug with evidence captured at that moment, re-ran repro steps before saving.
8. Final triage: severity, deduplication, sorting by module/severity.

## Results — 12 bugs

Full details in [`bug_tracker_Athul.csv`](bug_tracker_Athul.csv). Evidence links are public Drive links (verified viewable).

| ID | Title | Severity | Module | Evidence |
|----|-------|----------|--------|----------|
| BUG-001 | Bottom links do nothing (Contact Us, Privacy, Support, etc.) | P2 Major | Footer + Home help | [video](https://drive.google.com/file/d/1wFhfqs6kgF1Ci7_99-FYvbxKtW5pb1vd/view) |
| BUG-002 | Contact icon does nothing in Name field | P2 Major | Add Recipients | [video](https://drive.google.com/file/d/1j6_OIwtqXL4JDQjSrrczCgFNLVZmhFX6/view) |
| BUG-003 | Set signing order enabled with one recipient | P3 Minor | Add Recipients | [image](https://drive.google.com/file/d/1Apy6FQJAC9Hianc-dn2OIv5_HBpqQl5L/view) |
| BUG-004 | In Person Signer option is disabled | P2 Major | Add Recipients | [image](https://drive.google.com/file/d/1PIFWIDMdgvvEatQPBnaOtxjiJPU5II9T/view) |
| BUG-005 | Recipient customization options are disabled | P2 Major | Add Recipients → Customize | [image](https://drive.google.com/file/d/17USWzfvNfuthXZjbcNb395HwOhSXdlcy/view) |
| BUG-006 | Invalid email accepted, allows progression | P1 Critical | Add Recipients | [video](https://drive.google.com/file/d/15avfyxI-30coDr0dIl7pggJD3n6z8Dbf/view) |
| BUG-007 | Edit Recipients Save does not save | P1 Critical | Add Fields → Edit Recipients | [video](https://drive.google.com/file/d/19oc_VcU5NiQ4xVkOKoWwNiqWiV1fw2G3/view) |
| BUG-008 | Advanced Search missing owner/recipient filters | P2 Major | Templates → Advanced Search | [image](https://drive.google.com/file/d/18af2rAH-U9JT1R44leqxYkmM1hmVK7ZQ/view) |
| BUG-009 | Save dialog leaves Template name empty | P4 Trivial | Create Template | [image](https://drive.google.com/file/d/1u3iAEdM3vH3tu6DsCRU58BJsNPYnZn0k/view) |
| BUG-010 | Top menu Reports and Admin cannot be clicked | P2 Major | Top menu | [video](https://drive.google.com/file/d/1pgMvmD7iWCjS0AXhoY38_IhUIGOeUmzZ/view) |
| BUG-011 | Tasks and Help icons cannot be clicked | P3 Minor | Header | [video](https://drive.google.com/file/d/1hwjRDl04KvYNvKTTF_z7-w64HMUXGjVg/view) |
| BUG-012 | Same email can be added twice and sent | P1 Critical | Add Recipients → Manage | [video](https://drive.google.com/file/d/12vch56f6lQ1VCaI5ygH4OTlEvA7C9kRW/view) |

Severity spread: **P1 ×3, P2 ×6, P3 ×2, P4 ×1** — each row is one root cause, 5/5 reproducibility unless noted.

## Top 5 to fix first

1. **BUG-006 Invalid email accepted (P1)** — invalid recipient flows straight to Add Fields with no error. Data-integrity risk.
2. **BUG-007 Edit Recipients Save broken (P1)** — corrections can't be saved, dialog just sits there.
3. **BUG-012 Duplicate email accepted (P1)** — same email twice sends without warning, unlike original.
4. **BUG-001 Footer/help links dead (P2)** — Contact Us, Privacy, Support, Community, View Plans all do nothing.
5. **BUG-005 Customization options disabled (P2)** — access code, private message, advanced settings unavailable.

Overall impression: the core envelope workflow exists and looks close to the original, but validations and several key controls are incomplete. The main risk is users progressing without validation or hitting dead ends, not cosmetics. See [`QA_Summary_Note.pdf`](QA_Summary_Note.pdf) for the half-page summary.

## Repo structure

```text
.
├── bug_tracker_Athul.csv   # one row per bug, all columns + public evidence links
├── QA_Summary_Note.pdf     # overall impression, top 5, coverage, assumptions
├── evidence/               # local copies, BUG-00X_<module>_<slug>.png/.mp4
└── README.md
```

Evidence naming: `BUG-00X_<module>_<short-slug>` matches the Bug ID in the tracker. Videos cover sequences/timing, images cover static states.

## How to verify a bug

1. Use Chrome at 1280×960, 100% zoom.
2. Pick a row in `bug_tracker_Athul.csv`, follow Steps to Reproduce from a logged-in state.
3. Compare Expected (original DocuSign behavior) vs Actual.
4. Open the Evidence link for the recording/screenshot.

Notes: some disabled controls (Reports/Admin, header actions, some recipient types) look unimplemented rather than regressed — reported only where original behavior + visible UI made the defect clear. Branding, account-specific data, and plan-dependent differences were not logged without stronger evidence.
