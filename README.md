# Sprayer Calculator | RanchAssist™

> Calculate tank mixes, coverage, calibration, refills, product quantities, and application cost per acre.

**Live app:** [https://sprayercalculator.ranchassist.com](https://sprayercalculator.ranchassist.com)  
**RanchAssist tools:** [View Ranch Tools](https://ranchassist.com/tools)  
**Tool ID:** `sprayer-calculator`  
**Category:** Land

## Overview

A field-oriented pasture sprayer utility for tank-mix planning, coverage, calibration, refill planning, and application-cost calculations. The user supplies the applicable labeled rate; RanchAssist performs the arithmetic and planning calculations.

Sprayer Calculator is one of the standalone tools in the RanchAssist ecosystem. The product is designed around a focused workflow: open the tool for the ranch job in front of you, enter or map the information that matters, review assumptions and calculations, and leave with a result that can be saved, exported, printed, copied, or shared.

The tool is intended to feel like practical field-operations software rather than a general ranch-management platform. It does not require a RanchAssist account and does not depend on a shared RanchAssist project database.

## Key capabilities

- Mix Calculator for acres, target GPA, tank size, and repeatable product rows.
- Support user-entered rates in oz/acre, pt/acre, qt/acre, gal/acre, oz/100 gal, percent solution, or custom units.
- Calculate total carrier, total product, product/tank, full tanks, refills, and exact partial-final-tank amounts.
- Coverage Calculator for acres/tank and tanks/project.
- Sprayer Calibration using boom width or nozzle spacing/count, travel speed, nozzle flow, or catch-test measurements.
- Calculate GPA, acres/hour, carrier/hour, and tank duration.
- Cost Per Acre mode using container size/price, product cost, additives/carrier, labor, equipment, and fuel.
- Provide a compact per-tank instruction card suitable for viewing beside the sprayer.
- Show formulas and a prominent label-compliance notice.

## Typical workflow

1. Choose Mix, Coverage, Calibration, or Cost Per Acre mode.
2. Enter field acres, target GPA, and tank capacity as applicable.
3. Add products and enter the rate exactly from the applicable label.
4. If calibrating, enter boom/nozzle/speed/flow or catch-test measurements.
5. Enter container pricing and operational costs if estimating cost per acre.
6. Review per-tank quantities, refill plan, calibration outputs, and job cost.
7. Save the project or export/share the mix sheet.

## Calculation / measurement model

- Final spray solution = acres × target gallons per acre.
- Coverage per tank = tank gallons ÷ target GPA.
- Tank loads = total solution ÷ tank capacity, rounded up for the number of loads.
- Product requirements are calculated from the user-entered rate structure and project/tank volume.
- Calibration outputs derive GPA and field capacity from the selected boom/nozzle/speed/flow or catch-test inputs.
- Cost per acre allocates user-entered product, additive/carrier, labor, equipment, and fuel costs across treated acres.

Important formulas and assumptions should be available in the UI through **Calculation Details** or the equivalent audit view. RanchAssist favors transparent calculations over opaque outputs, and user-editable defaults should be treated as planning examples rather than universal recommendations.

## Project state

A saved RanchAssist `.ra` file for this tool should preserve enough information to resume meaningful editing, including active mode, field inputs, tank inputs, repeatable products/rates, container size and price, calibration inputs, cost inputs, planning assumptions, notes, and project metadata.

Every `.ra` project should include the shared RanchAssist project metadata: project UUID, project name, originating tool ID, tool version, `.ra` format version, `createdAt`, `updatedAt`, editable state, and spatial state where applicable. After opening a project, derived values should be recalculated using the current application logic rather than blindly trusting previously saved calculated totals.

## Export and sharing

Supported/expected outputs for this tool include:

- `.ra` project file
- PDF / print mix sheet
- CSV product table
- Compact per-tank instruction card
- Copy Summary
- Email summary

Report-oriented outputs should include the tool name, project name when supplied, important inputs, assumptions, calculations, key results, warnings/notes, and a timestamp. Spatial reports should include the relevant map or plan image when practical.

## Project files, session data, and privacy

RanchAssist uses a **session-first, user-controlled persistence model**.

- No RanchAssist account is required for the standalone tool.
- `sessionStorage` may protect active work from an accidental refresh or same-tab interruption.
- `localStorage` is not used as permanent project storage.
- RanchAssist does not permanently store the user's ranch/project data by default.
- **Save Project** creates a user-controlled `.ra` RanchAssist Project File containing the editable project state.
- **Open Project** restores a compatible `.ra` file after validation.
- **Duplicate Project** creates a new project lineage with a new project UUID.
- **Clear Session / Start Over** clears active browser-session data without deleting `.ra` files or exports already saved on the user's device.

A `.ra` file is distinct from report/data exports. PDF, CSV, PNG, GeoJSON, print output, email, and copied summaries are for communication or interoperability; `.ra` is for resuming the editable project.

Imported project files are data, not executable code. The application must not execute imported JavaScript, use `eval`, restore secrets, or inject unsanitized imported HTML.


## Runtime configuration

The core Sprayer Calculator does not require Mapbox or another third-party API. Email sharing uses Apps Script `MailApp`. No end-user API-key fields should exist.

RanchAssist administrators configure developer infrastructure; normal users should never be asked to paste API keys, Mapbox tokens, OAuth secrets, or other developer credentials into the tool.

## Architecture

This repository follows the RanchAssist standalone-tool architecture:

- **Primary deployment:** Google Apps Script Web App.
- **Frontend:** semantic HTML5, componentized CSS, and lightweight client-side JavaScript.
- **Dependencies:** kept intentionally minimal so the tool remains maintainable and field-friendly.
- **Responsive behavior:** desktop, tablet, and phone are treated as distinct layouts rather than a desktop interface simply being scaled down.
- **Accessibility:** real form labels, keyboard-operable controls, visible focus states, plain-language validation, WCAG-AA-minded contrast, and large mobile touch targets.
- **Server responsibilities:** server-only operations such as email sharing or private API calls stay in Apps Script rather than exposing secrets in the browser.

The application should remain independently usable and deployable. It does not depend on a shared RanchAssist account system or a cross-tool project database.


## Suggested repository structure

The exact filenames may vary by release, but a RanchAssist repository should stay intentionally small and understandable. A common Apps Script layout is:

```text
.
├── Code.gs                 # Apps Script backend / runtime configuration / email share
├── Index.html              # Main UI (often self-contained HTML + CSS + client JS)
├── appsscript.json         # Apps Script manifest, when managed in source
├── README.md               # Repository documentation
└── README_SETUP.txt        # Deployment/configuration notes when distributed as source files
```

Some releases may include separate style/app partials or a standalone HTML build when that materially improves maintainability or portability. Keep secrets out of all client-side files.


## Google Apps Script deployment

A typical deployment flow is:

1. Create or open the Google Apps Script project for this repository.
2. Add the repository's Apps Script backend and HTML frontend files.
3. Configure any required **Script Properties** listed in the Runtime Configuration section below.
4. Save the project.
5. Choose **Deploy → New deployment → Web app**.
6. Select the execution/access settings appropriate for the RanchAssist deployment.
7. Authorize required Apps Script services, such as `MailApp`, when email sharing is enabled.
8. Deploy and verify the public RanchAssist subdomain routes to the current web-app deployment.

Do not put private API keys directly in frontend HTML or JavaScript. Script Properties are the canonical configuration store for Apps Script deployments.


## Design system

The UI follows the RanchAssist visual standard: modern field-operations software, monochrome-first surfaces, warm off-white canvas, near-black text, thin borders, restrained functional color, compact controls, strong alignment, minimal decorative effects, and responsive layouts designed for real use from a phone, tablet, pickup, barn, pasture, or ranch office.

Key UX expectations include:

- Results should be understandable quickly.
- Units should always be explicit.
- Assumptions should appear near the inputs they affect.
- Advanced settings should stay out of the way of the primary workflow.
- Validation should explain both the problem and how to fix it.
- Destructive actions such as Clear Session should require an appropriate confirmation when meaningful work exists.
- Status should never rely on color alone.
- Mobile layouts should favor large controls, limited typing, stacked sections/bottom sheets, and early visibility of the primary result.

## Safety and limitations

The pesticide or herbicide label is the controlling source for legal application rate, PPE, restrictions, re-entry, grazing/haying intervals, environmental precautions, and tank-mixing requirements. RanchAssist does not recommend products or off-label rates; users must enter the applicable label rate themselves.

RanchAssist should use language such as **estimate**, **planning estimate**, **projected requirement**, **mapped measurement**, and **user-defined scenario**. Avoid presenting results as guaranteed, certified, surveyed, engineered, legally compliant, veterinarian-approved, or otherwise authoritative beyond what the user's inputs and the tool's calculations support.

## QA checklist

Before publishing a release, verify at minimum:

- [ ] Core calculations or map interactions work on desktop, tablet, and phone.
- [ ] Required, Optional, Advanced, and Assumption inputs are clearly identified where applicable.
- [ ] Units are always visible and conversions are consistent.
- [ ] Invalid values produce a clear explanation and a corrective action.
- [ ] Calculation details/formulas are available where the tool performs calculations.
- [ ] `.ra` Save Project creates a valid project file.
- [ ] A saved `.ra` file can be opened after Clear Session and restores editable state.
- [ ] Wrong-tool, corrupt, or unsupported `.ra` files fail safely.
- [ ] Derived results are recalculated after a project is reopened.
- [ ] Refresh recovery works through `sessionStorage` without implying permanent saving.
- [ ] Clear Session does not delete local `.ra` or report exports.
- [ ] Print/PDF output is readable and includes key assumptions/results.
- [ ] CSV output opens cleanly in standard spreadsheet software when CSV is supported.
- [ ] Copy Summary produces concise, useful plain text.
- [ ] Email sharing works without exposing server-side credentials.
- [ ] No private credential appears in page source, browser storage, `.ra` files, exports, or logs.
- [ ] Mobile controls meet practical touch-target and field-use requirements.


## Development principles

When extending this repository:

1. Keep the tool focused on its core ranch job.
2. Preserve the no-account, standalone architecture unless an approved RanchAssist source-of-truth explicitly changes it.
3. Keep project persistence user-controlled through `.ra` files rather than adding silent cloud or long-term browser storage.
4. Keep deployment configuration separate from user project state.
5. Never serialize secrets or executable code into `.ra` files.
6. Use an explicit allowlist when exposing browser-safe runtime configuration.
7. Keep calculations auditable and assumptions editable.
8. Recalculate derived values after restoring saved projects.
9. Design mobile behavior intentionally rather than shrinking desktop layouts.
10. Update this README whenever major capabilities, configuration requirements, project-state schema, or public URLs change.

## RanchAssist ecosystem

- [View Ranch Tools](https://ranchassist.com/tools) — **RanchAssist.com**
- [UnScriptly](https://unscriptly.com/)
- [Designed and developed by Mark Allan](https://bymarkallan.com/)
- [A Mahtco company](https://mahtco.com/)
- [Designed in Plano, TX by Qalori ❤️](https://qalori.com/)

---

**RanchAssist™** builds focused calculators, estimators, planners, and mapping utilities for practical ranch work. Results produced by RanchAssist are planning outputs based on user-entered information and assumptions; they are not guarantees or professional certifications.



Source mirror managed by GitScript.com
