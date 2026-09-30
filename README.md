# DocuSign Clone — QA Bug Hunt, Triage & Reporting

Comparative QA pass of a DocuSign clone against the original DocuSign app, focused on what matters most to users: pixel-accurate UI fidelity and functional flows that hold up end to end.

I tested side-by-side at a fixed viewport, logged every defect with blind-reproducible steps, triaged by severity, and attached video/image evidence for each bug.

## Environment

- **Browser:** Chrome 152.0.7977.84, 1280×960, 100% zoom
- **OS:** macOS 26.2.2
- **Reference:** DocuSign free trial account (source of truth)
- **Target:** DocuSign clone
- **Tools:** Chrome DevTools (spacing, computed font weight, hex colors, console), pixel-overlay extension, screenshot + screen recorder

All bugs in `bug_tracker_Athul.csv` are reproducible in this environment (5/5 attempts unless noted).

## Scope

**Tested:**
- Pixel-level UI issues: alignment, spacing, color, font weight, icon size, separators, container sizing/padding
- Functional flows: broken states, wrong navigation, form validation, error handling, filters, templates
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
3. Built a module checklist: auth/landing, dashboard, upload, recipients/routing, field placement, send/confirmation, manage/status, agreements/filters, templates, global nav/modals/toasts.
4. Pixel pass per screen with overlay: spacing, font weight, colors, icon size, separators, container dimensions, hover/focus/disabled/loading/empty states.
5. Functional pass per module: happy path, validation (required, invalid email, duplicates), navigation (back, refresh, direct URL), filters/advanced search, error handling.
6. Edge push: invalid/duplicate emails, single-recipient signing order, disabled options, dead links/icons, discard flow destination, filter defaults.
7. Logged one row per bug with evidence captured at that moment, re-ran repro steps before saving.
8. Final triage: severity, deduplication, sorting by module/severity.

## Results — 45 bugs

Full details in [`bug_tracker_Athul.csv`](bug_tracker_Athul.csv). Evidence files are local in [`evidence/`](evidence/).

| ID | Title | Severity | Module | Evidence |
|----|-------|----------|--------|----------|
| BUG-001 | Bottom links do nothing (Contact Us, Privacy, Support, etc.) | P2 | Bottom footer + Home help boxes | `evidence/BUG-001_footer_links-unresponsive.mp4` |
| BUG-002 | Contact icon does nothing in Name field | P2 | Get Signatures → Add Recipients | `evidence/BUG-002_unresponsive_contact_icon.mp4` |
| BUG-003 | Set signing order enabled with one recipient | P3 | Get Signatures → Add Recipients | `evidence/BUG-003_signing-order-enabled-one-recipient.png` |
| BUG-004 | In Person Signer option is disabled | P2 | Get Signatures → Add Recipients | `evidence/BUG-004_add-recipients_in-person-signer-disabled.png` |
| BUG-005 | Recipient customization options are disabled | P1 | Get Signatures → Add Recipients → Customize | `evidence/BUG-005_add-recipients_customize-options-disabled.png` |
| BUG-006 | Invalid email accepted and allows progression | P0 | Get Signatures → Add Recipients | `evidence/BUG-006_add-recipients_invalid-email-accepted.mp4` |
| BUG-007 | Edit Recipients Save does not save changes | P0 | Add Fields → Edit Recipients | `evidence/BUG-007_edit-recipients_save-button-unresponsive.mp4` |
| BUG-008 | Templates: Advanced Search missing owner and recipient filters | P2 | Templates → My Templates → Advanced Search | `evidence/BUG-008_templates_advanced-search-filters-missing.png` |
| BUG-009 | Save dialog leaves Template name empty | P4 | Start → Envelope Templates → Create a Template | `evidence/BUG-009_clone-save-dialog-empty-name.png` |
| BUG-010 | Top menu Reports and Admin cannot be clicked | P2 | Top menu | `evidence/BUG-010_menu_reports-admin-faded.mp4` |
| BUG-011 | Tasks and Help icons cannot be clicked | P2 | Top right header | `evidence/BUG-011_header_tasks-help-disabled.mp4` |
| BUG-012 | Same email can be added twice and the file is sent | P1 | Get Signatures → Add Recipients → Manage | `evidence/BUG-012_send_duplicate-email-twice.mp4` |
| BUG-013 | Home – Start button size differs from original | P3 | Home / Dashboard | `evidence/BUG-013_home_start-button-size-mismatch.mov` |
| BUG-014 | Home – Primary action buttons differ from original | P2 | Home / Dashboard | `evidence/BUG-014_home-primary-actions-mismatch.mov` |
| BUG-015 | Start > Envelopes submenu contains an extra “Send with AI” option | P3 | Home / Start → Envelopes | `evidence/BUG-015_envelopes-submenu-extra-send-with-ai.mov` |
| BUG-016 | Envelope Templates submenu has incorrect container size | P3 | Home → Start → Envelope Templates | `evidence/BUG-016_envelope-templates-submenu-container-size.mov` |
| BUG-017 | DocuSign logo has incorrect left-side spacing | P2 | Global Header / Home | `evidence/BUG-017_header-logo-left-spacing..mov` |
| BUG-018 | Help notification badge has incorrect positioning | P4 | Global Header / Home | `evidence/BUG-018_help-badge-position.mov` |
| BUG-019 | Upgrade banner displays different heading and description | P3 | Home / Upgrade banner | `evidence/BUG-019_upgrade-banner-text-mismatch.mov` |
| BUG-020 | Upgrade banner View Plans link does not respond | P2 | Home / Upgrade banner | `evidence/BUG-020_view-plans-no-response.mov` |
| BUG-021 | “View Our Guide” link does not respond | P2 | Home / Getting Started guide banner | `evidence/BUG-021_view-our-guide-no-response.mov` |
| BUG-022 | “Join our Product Experience Research Panel” link does not respond | P3 | Home / Footer | `evidence/BUG-022_research-panel-link-no-response.mov` |
| BUG-023 | Footer Container support links do not respond | P2 | Home / Footer | `evidence/BUG-023_footer-support-links-no-response.mov` |
| BUG-024 | Footer language selector does not respond | P2 | Home / Footer | `evidence/BUG-024_footer-language-selector-no-response.mov` |
| BUG-025 | Footer copyright text has incorrect alignment | P3 | Home / Footer | `evidence/BUG-025_footer-copyright-alignment.png` |
| BUG-026 | Footer links have incorrect horizontal spacing | P4 | Home / Footer | `evidence/BUG-026_footer-link-spacing.png` |
| BUG-027 | Footer container has incorrect padding | P4 | Home / Footer | `evidence/BUG-027_footer-container-padding.png` |
| BUG-028 | Extra separator appears before Contact Us in footer | P4 | Home / Footer | `evidence/BUG-028_footer-extra-separator-before-contact-us.png` |
| BUG-029 | Footer separators have incorrect color/contrast | P4 | Home / Footer | `evidence/BUG-029_footer-separator-color-mismatch.png` |
| BUG-030 | Missing separator before Trust link in footer | P3 | Home / Footer | `evidence/BUG-030_footer-missing-separator-before-trust.png` |
| BUG-031 | Send with AI button does not respond when clicked | P2 | Home / Send with AI | `evidence/BUG-031_send-with-ai-button-no-response.mov` |
| BUG-032 | Help, Advanced Options and View Plans controls do not respond in Get Signatures | P2 | Get Signatures / Envelope Setup | `evidence/BUG-032_get-signatures-top-right-controls-non-responsive.mov` |
| BUG-033 | New navigation toggle does not enable in clone | P2 | Agreements / Navigation | `evidence/BUG-033_new-navigation-toggle-not-working.mov` |
| BUG-034 | Authentication Failed navigation does not respond when clicked | P2 | Agreements / Left Navigation | `evidence/BUG-034_authentication-failed-navigation-no-response.mov` |
| BUG-035 | Bulk Send navigation does not respond when clicked | P2 | Agreements / Left Navigation | `evidence/BUG-035_bulk-send-navigation-no-response.mov` |
| BUG-036 | PowerForms navigation does not respond when clicked | P2 | Agreements / Left Navigation | `evidence/BUG-036_powerforms-navigation-no-response.mov` |
| BUG-037 | Discarding an unsaved envelope redirects to Deleted instead of Inbox | P2 | Get Signatures / Discard Changes | `evidence/BUG-037_discard-envelope-redirects-to-deleted.mov` |
| BUG-038 | Clear button is visible when no filters are applied | P3 | Agreements / Inbox, Sent and other agreement lists | `evidence/BUG-038_clear-button-visible-without-filters.mov` |
| BUG-039 | Date filter options differ from original DocuSign | P2 | Agreements → Inbox → Date filter | `evidence/BUG-039_agreements_date-filter-options.mov` |
| BUG-040 | Status filter options differ from original DocuSign | P2 | Agreements → Inbox → Status filter | `evidence/BUG-040_agreements_status-filter-options.mov` |
| BUG-041 | Sender filter options differ from original DocuSign | P2 | Agreements → Inbox → Sender filter | `evidence/BUG-041_agreements_sender-filter-options.mov` |
| BUG-042 | Advanced Search filter options differ from original DocuSign | P2 | Agreements → Inbox → Advanced search | `evidence/BUG-042_agreements_advanced-search-options.mov` |
| BUG-043 | Workflow Templates and Template Gallery navigation items are unresponsive | P2 | Templates → Navigation sidebar | `evidence/BUG-043_templates_unresponsive-navigation.mov` |
| BUG-044 | Advanced Search options differ from original DocuSign | P2 | Templates → My Templates → Advanced search | `evidence/BUG-044_templates_advanced-search-options.mov` |
| BUG-045 | Column customization options differ from original DocuSign | P2 | Templates → My Templates → Column customization | `evidence/BUG-045_templates_column-customization.mov` |

Severity spread: **P0 ×2, P1 ×2, P2 ×26, P3 ×9, P4 ×6** — each row is one root cause, 5/5 reproducibility unless noted. The bug tracker is the source of truth for current severity levels.

## Top 5 to fix first

1. **BUG-006 Invalid email accepted (P0)** — malformed recipient email is accepted and the flow continues, making recipient validation unreliable.
2. **BUG-007 Edit Recipients Save broken (P0)** — recipient edits are not persisted, breaking a core envelope workflow.
3. **BUG-005 Customization options disabled (P1)** — access code, private message, advanced settings unavailable in recipient setup.
4. **BUG-012 Duplicate email accepted (P1)** — same email twice sends without warning, unlike original.
5. **BUG-001 Footer/help links dead (P2)** — several user-facing support/navigation links do nothing across Home entry points.

Overall impression: the clone covers the main DocuSign-style envelope workflow and is broadly usable, but the latest pass found gaps in validation, recipient controls, navigation, filtering, and UI fidelity. The main risk is users progressing without validation or hitting dead ends, plus filter/navigation drift from the original. See [`summary_note.pdf`](summary_note.pdf) for the half-page summary.

## Repo structure

```text
.
├── bug_tracker_Athul.csv   # one row per bug, all columns + local evidence refs
├── summary_note.pdf        # overall impression, top 5, coverage, assumptions
├── evidence/               # local copies, BUG-00X_<module>_<slug>.png/.mp4/.mov
└── README.md
```

Evidence naming: `BUG-00X_<module>_<short-slug>` matches the Bug ID in the tracker. Videos (`.mp4`/`.mov`) cover sequences/timing, images (`.png`) cover static states.

## How to verify a bug

1. Use Chrome at 1280×960, 100% zoom.
2. Pick a row in `bug_tracker_Athul.csv`, follow Steps to Reproduce from a logged-in state.
3. Compare Expected (original DocuSign behavior) vs Actual.
4. Open the matching file in `evidence/` for the recording/screenshot.

Notes: some disabled controls (Reports/Admin, header actions, some recipient types) look unimplemented rather than regressed — reported only where original behavior + visible UI made the defect clear. Branding, account-specific data, and plan-dependent differences were not logged without stronger evidence.
