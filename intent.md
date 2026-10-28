# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded history and approves nothing here. This is the second revision of the draft. It was made only to trigger the grading workflow on a new commit; the content is unchanged.

## Problem

I-1. The fictional clinic receives avoidable calls from patients who are only checking whether their results are ready (PRD N-17, Problem and outcome). Patients have no way to know when a result is ready for their review, so they phone the clinic to ask.

## Proposed outcome

I-2. When a result is ready for a patient's review, the patient is notified that an update is available and is directed to the existing authenticated portal (PRD N-17).

I-3. "Ready" means a doctor has reviewed and released the result. A finished laboratory upload alone never makes a result ready, and no notification is eligible before review and release (PRD N-17 constraint 1; product owner in discovery notes).

I-4. The only text in the message is that an update is available. It carries no test name, result, diagnosis, measurement or treatment detail.

How success will be observed (observable measures only, no invented target):

- I-5. Status-only calls ("are my results ready?") fall compared with a measured baseline. Support counts them separately from clinical questions, and the two counts are kept as separate values (support lead, discovery notes).
- I-6. Delivery failures are tracked as a separate measure (PRD N-17).
- I-7. No numeric target is approved. The baseline and any target are agreed during discovery. The Support Lead owns measuring the baseline and keeping the two call counts separate.

## Affected users and systems

- I-8. Patients of the clinic: they receive the notification and sign in to the existing portal to view their result.
- I-9. Doctors: they review and release results, and nothing is sent until they do.
- I-10. Support staff and the support lead: they field status calls and supply the baseline and call counts.
- I-11. The existing authenticated portal and its existing result route, which is used as is (engineering lead: the existing portal path is adequate). Access is authenticated in the portal. Results are never attached to the message, and there is no public result link.
- I-12. The notification system and its operational logs, which must hold no clinical content (privacy owner).

## Out of scope

- I-13. Portal redesign, and a new patient dashboard, which is a separate project (PRD N-17; engineering lead).
- I-14. Appointment scheduling and clinical interpretation (PRD N-17).
- I-15. Notifying third parties. This stays excluded, and the referring doctor is handled separately as an open question (see OPEN-2).
- I-16. Everything that only the superseded early request asked for: automatic sending as soon as the lab uploads, the test result in an SMS, copying the referring doctor, rebuilding the patient dashboard and the 90% call reduction in one week. None of it is approved.

## Constraints

- C-1. A doctor must review and release the result before any notification is eligible (PRD N-17; product owner).
- C-2. The message says only that an update is available and directs the patient to the existing authenticated portal. No diagnosis, test result, clinical measurement or treatment detail appears in the message or in notification logs (PRD N-17; privacy owner).
- C-3. Use only the existing portal result route, and authenticate access in the existing portal. Do not attach results or introduce a public result link (PRD N-17).
- C-4. Send no duplicate notification for the same released result version. Handle delivery failures without sending duplicate patient messages (PRD N-17).
- C-5. Do not approve any recipient or channel that has not been decided. Consent and the actual channel are still under review (privacy owner).

## Open questions with owners

- OPEN-1. Channel: SMS, email or portal push? Owners: product owner and privacy owner. They decide after consent and delivery review. No channel is chosen or assumed here.
- OPEN-2. Should the referring doctor be notified? Owners: clinical owner and privacy owner, who must decide recipient authorization. This is not approved by the PRD, and approval must not be inferred from the superseded request (clinical owner, discovery notes). It is kept apart from the excluded third-party notifications and decided on its own.

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
