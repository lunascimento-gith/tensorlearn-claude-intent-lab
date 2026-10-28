# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17 and is used as history only; nothing in this intent is approved by it alone.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their results are ready. PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready."

## Proposed outcome

- **I-2** The patient is notified when a result is ready for their review in the existing authenticated portal. PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal."
- **I-3** "Ready" means a doctor has reviewed and released the result. A completed laboratory upload alone does not make a result ready. PRD: "A doctor must review and release the result before any notification is eligible." Product owner: "'Ready' must not mean that the laboratory upload merely finished."
- **I-4** How success will be observed:
  - a reduction in status-only calls about results, counted separately from clinical questions and compared against a measured baseline (PRD: "Measure reduction in results-status calls"; support lead: "count status-only calls separately from clinical questions; compare against a measured baseline");
  - delivery failures, which are measured (PRD: "and delivery failures").
- **I-5** No baseline or numeric target is approved. Both are to be agreed during discovery. PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet."

## Affected users and systems

- **I-6** Patients, who receive the notification and then sign in to the existing portal to see their result.
- **I-7** Doctors, whose review and release of a result is the event that makes a notification eligible.
- **I-8** Support staff, who handle results-status calls and whose call counts are used to measure success.
- **I-9** Systems: the existing authenticated portal and its existing result route, the result review and release step, and the patient notification delivery, including its logs.
- **I-10** Only the patient is approved as a recipient. Notifying the referring doctor is not approved (see OPEN-2).

## Out of scope

- **I-11** Portal redesign (PRD Scope).
- **I-12** Appointment scheduling (PRD Scope).
- **I-13** Clinical interpretation (PRD Scope).
- **I-14** Notifying third parties (PRD Scope).
- **I-15** A new or rebuilt patient dashboard, which is a separate project. Engineering lead: "A new patient dashboard is a separate project."
- **I-16** Excluded parts of the superseded request: sending every result as soon as the lab uploads it, including the test result in an SMS, copying the referring doctor, rebuilding the patient dashboard, and the "90% call reduction in one week" target.

## Constraints

- **C-1** A notification is eligible only after a doctor has reviewed and released the result. PRD: "A doctor must review and release the result before any notification is eligible."
- **C-2** The message says only that an update is available and directs the patient to the existing authenticated portal. PRD: "The notification says that an update is available and directs the patient to the existing authenticated portal."
- **C-3** No diagnosis, test result, clinical measurement or treatment detail appears in the message or in notification and operational logs. PRD: "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs." Privacy owner: "no sensitive content in messages or operational logs."
- **C-4** Access is authenticated in the existing portal. Results are not attached and no public result link is introduced. PRD: "Authenticate access in the existing portal. Do not attach results or introduce a public result link."
- **C-5** The existing portal result route is used. PRD: "Use the existing portal result route." Engineering lead: "the existing portal path is adequate."
- **C-6** No duplicate notifications are sent for the same released result version. PRD: "Avoid duplicate notifications for the same released result version."
- **C-7** Delivery failures are handled without sending the patient duplicate messages. PRD: "handle delivery failures without duplicate patient messages."

## Open questions

- **OPEN-1** Channel: should the notification go by SMS, email or portal push? Owners: product owner and privacy owner. They decide after the consent and delivery review. PRD: "Channel (SMS, email, portal push): product owner and privacy owner must decide after consent/delivery review." Privacy owner: "Consent and the actual channel are still under review." Not decided by this intent.
- **OPEN-2** Notifying the referring doctor: may the referring doctor be notified, and with what recipient authorization? Owners: clinical owner and privacy owner. PRD: "clinical owner and privacy owner must decide recipient authorization. It is not approved by this PRD." Clinical owner: "do not infer approval for referring-doctor notifications from the old request." Not approved, and not decided by this intent.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
