# Intent

Source basis: approved PRD N-17 (2026-09-18, `inputs/01-approved-prd.md`) and the discovery notes of 2026-09-20 (`inputs/02-current-meeting.md`). The early request of 2026-08-02 (`inputs/03-superseded-request.md`) is superseded history and approves nothing here.

## Problem

The fictional clinic receives avoidable calls from patients who only want to know whether their results are ready. Patients have no signal that a result has been reviewed and released, so they phone to ask. (PRD N-17, Problem and outcome.)

## Proposed outcome

- **I-1** A patient is notified when a result is ready for their review in the existing authenticated portal.
- **I-2** A notification is eligible only after a doctor has reviewed and released the result. A finished laboratory upload alone does not make a result "ready" (product owner, 2026-09-20).
- **I-3** The message says only that an update is available and directs the patient to the existing authenticated portal.

How success will be observed:
- **S-1** The count of status-only calls ("are my results ready?") falls against a baseline that is measured during discovery. Support counts these separately from clinical questions.
- **S-2** Delivery failures are tracked as a second observable measure.
- No baseline or numeric target is approved yet, so none is stated here. Agreeing both is part of discovery.

## Affected users and systems

- Patients receive the notification and read the result in the portal after authenticating.
- Doctors review and release results, and that release is the gate for any notification.
- Support staff handle status calls and measure their volume.
- Systems: the existing authenticated patient portal and its existing result route, plus the notification delivery path, whose channel is still open (OPEN-1).

## Out of scope

- **X-1** Portal redesign.
- **X-2** A new patient dashboard. The engineering lead calls it a separate project, and the existing portal path is adequate.
- **X-3** Appointment scheduling.
- **X-4** Clinical interpretation.
- **X-5** Notifying third parties. This includes the referring doctor, who is covered only as an open question (OPEN-2) and is not approved here.
- **X-6** Everything that only the superseded request asked for: sending on lab upload, putting the test result in an SMS, copying the referring doctor, rebuilding the dashboard and the 90% reduction in one week target.

## Constraints

- **C-1** A doctor must review and release the result before any notification is eligible.
- **C-2** No diagnosis, test result, clinical measurement or treatment detail goes in the message or in notification logs (privacy owner).
- **C-3** Access is authenticated in the existing portal. Results are never attached and there is no public result link.
- **C-4** At most one notification per released result version. Delivery failures are handled without sending duplicate patient messages.
- **C-5** The existing portal result route is used. The channel and consent are still under review and must not be assumed.

## Open questions and owners

- **OPEN-1** Channel (SMS, email or portal push). Owners: product owner and privacy owner, after the consent and delivery review.
- **OPEN-2** Whether to notify the referring doctor, which requires recipient authorization. Owners: clinical owner and privacy owner. It is not approved, and approval must not be inferred from the old request.
- Baseline and numeric target for call reduction: to be agreed in discovery. Nothing is approved yet.

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
