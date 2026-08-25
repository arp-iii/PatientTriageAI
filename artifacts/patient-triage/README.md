# PatientTriage.ai

PatientTriage.ai is Team Eclipse's hackathon prototype for an explainable,
AI-assisted emergency-department triage cockpit. It is a frontend-only
simulation: all patients are fictional, all scoring runs locally, and a
clinician remains responsible for the final queue decision.

## Setup

```bash
cd artifacts/patient-triage
npm install
npm run dev
```

The app is also part of the workspace. From the repository root:

```bash
pnpm --filter @workspace/patient-triage run typecheck
```

## Architecture

- `src/App.tsx` contains the demo data model, local scoring engine, shared
  shell, routes, intake flow, queue, patient drawer, and analytics surfaces.
- `localStorage` persists seeded-queue changes under
  `patient-triage-patients`, intake drafts under
  `patient-triage-intake-draft`, and the light/dark preference under
  `patient-triage-theme`.
- The scoring engine is deterministic and intentionally readable. It combines
  complaint keywords, risk factors, abnormal vitals, age, consciousness,
  missing-data confidence penalties, and waiting-time context. Incomplete
  records surface a `Needs clinician review` safeguard.
- The clinician drawer writes human overrides and reasons to each patient's
  audit log. Reset restores the original fictional shift.
- Recharts powers the triage distribution view, while the supporting wait-time
  and calibration visuals use deliberately lightweight local chart blocks so
  the demo stays fast and self-contained. Every chart is driven by the same
  seeded simulation metrics shown in the command center.

No backend or external API is required for the prototype. Do not use this
interface for real clinical decisions.

## 90-second presentation script

**0:00–0:15 — Set the scene.** “In an emergency department, the first minute
contains the most important signal, but it is also the noisiest. PatientTriage.ai
turns that first handoff into a structured, reviewable record.”

**0:15–0:35 — Show intake.** Open **Nurse intake**, choose **Load demo
shortcut**, move through the four stages, and point out autosave, missing-data
warnings, and the review screen. Submit the fictional case.

**0:35–1:05 — Show the queue.** Handoff to **Command center**. Open Mara
Ellison or the newly submitted case. Expand **Why this decision?** to show
narrative, vitals, and confidence signals. Point out that the score is a
suggestion, not a diagnosis.

**1:05–1:20 — Show the human gate.** Choose **Review / override**, select a
different level, enter a reason, and save. The override appears in the audit
trail: the system records what the clinician changed and why.

**1:20–1:30 — Close with trust.** Open **Architecture** and finish on
**Analytics**. “The memorable part is not that a model produces a number. It
is that every number can be understood, challenged, and owned by a clinician.”